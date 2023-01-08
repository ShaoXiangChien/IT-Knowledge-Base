## Infrastrucutre As Code
- refer to mlops workshop v1
- [iac-create-environment-pipeline-arm.yml](https://github.com/ShaoXiangChien/MLOpsPython/blob/master/environment_setup/iac-create-environment-pipeline-arm.yml)
- 
- [ ] main pipeline
``` yml
# Copyright (c) Microsoft Corporation. All rights reserved.

# Licensed under the MIT License.

  

variables:

- ${{ if eq(variables['Build.SourceBranchName'], 'main') }}:

    # 'main' branch: PRD environment

    - template: ../../config-infra-prod.yml

- ${{ if ne(variables['Build.SourceBranchName'], 'main') }}:  

    # 'develop' or feature branches: DEV environment

    - template: ../../config-infra-dev.yml

- name: version

  value: aml-cli-v2 

- name: endpoint_name

  value: taxi-batch-$(namespace)$(postfix)$(environment)

- name: endpoint_type

  value: batch

  

trigger:

- none

  

pool:

  vmImage: ubuntu-20.04

  
  

resources:

  repositories:

    - repository: mlops-templates  # Template Repo

      name: ShaoXiangChien/mlops-templates # need to change org name from "Azure" to your own org

      endpoint: github-connection # need to set up and hardcode

      type: github

      ref: main

  

stages:

- stage: CreateBatchEndpoint

  displayName: Create/Update Batch Endpoint 

  jobs:

    - job: DeployBatchEndpoint

      steps:

      - checkout: self

        path: s/

      - checkout: mlops-templates

        path: s/templates/

      - template: templates/${{ variables.version }}/install-az-cli.yml@mlops-templates

      - template: templates/${{ variables.version }}/install-aml-cli.yml@mlops-templates

      - template: templates/${{ variables.version }}/connect-to-workspace.yml@mlops-templates

      - template: templates/${{ variables.version }}/create-compute.yml@mlops-templates

        parameters:

          cluster_name: batch-cluster # name must match cluster name in deployment file below

          size: STANDARD_DS3_V2

          min_instances: 0

          max_instances: 5

          cluster_tier: dedicated

      - template: templates/${{ variables.version }}/create-endpoint.yml@mlops-templates

        parameters: 

          endpoint_file: mlops/azureml/deploy/batch/batch-endpoint.yml

      - template: templates/${{ variables.version }}/create-deployment.yml@mlops-templates

        parameters:

          deployment_name: diabetes-batch-dp

          deployment_file: mlops/azureml/deploy/batch/batch-deployment.yml      

      - template: templates/${{ variables.version }}/test-deployment.yml@mlops-templates

        parameters:

          deployment_name: diabetes-batch-dp

          sample_request: data/taxi-batch.csv

          request_type: uri_file #either uri_folder or uri_file
```
- [ ] deploy model with aml cli v2
1. create endpoint
  ```
  # Copyright (c) Microsoft Corporation. All rights reserved.
	# Licensed under the MIT License.
	
	parameters: 
	- name: endpoint_file
	  type: string
	
	steps:
	  - task: AzureCLI@2
	    displayName: Create online/batch endpoint 
	    continueOnError: true
	    inputs: 
	      azureSubscription: $(ado_service_connection_rg) #needs to have access at the RG level 
	      scriptType: bash
	      scriptLocation: inlineScript
	      inlineScript: |
	        ENDPOINT_EXISTS=$(az ml $(endpoint_type)-endpoint list -o tsv --query "[?name=='$(endpoint_name)'][name]" | wc -l)
	        echo $ENDPOINT_NAME $ENDPOINT_EXISTS
	        az ml $(endpoint_type)-endpoint list -o tsv
	
	        if [[ ENDPOINT_EXISTS -ne 1 ]]; then
	            az ml $(endpoint_type)-endpoint create --name $(endpoint_name) -f ${{ parameters.endpoint_file }}
	        else
	            echo "Endpoint exists"
	        fi
  ```
- batch-endpoint.yml
```
$schema: https://azuremlschemas.azureedge.net/latest/batchEndpoint.schema.json
name: diabetes-batch
description: diabetes batch endpoint
auth_mode: aad_token
```

2. Create deployment
```
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

parameters:
- name: deployment_name
  type: string
- name: deployment_file
  type: string
- name: enable_monitoring
  type: string
  default: 'false'

steps:
  - task: AzureCLI@2
    displayName: Create deployment
    continueOnError: true
    inputs: 
      azureSubscription: $(ado_service_connection_rg) #needs to have access at the RG level 
      scriptType: bash
      scriptLocation: inlineScript
      inlineScript: |
        set -o xtrace
        az ml $(endpoint_type)-deployment create --name ${{ parameters.deployment_name }} --endpoint $(endpoint_name) \
          -f ${{ parameters.deployment_file }}
```
- deployment.yml
```
$schema: https://azuremlschemas.azureedge.net/latest/batchDeployment.schema.json
name: batch-dp
endpoint_name: diabetes-batch
model: azureml:diabeteDesignerModel@latest
compute: azureml:batch-cluster
resources:
  instance_count: 1
max_concurrency_per_instance: 2
mini_batch_size: 10
output_action: append_row
output_file_name: predictions.csv
retry_settings:
  max_retries: 3
  timeout: 30
error_threshold: -1
logging_level: info
```
- scoring script (script for the endpoint to run while called)
``` python
import os

import json

  

from azureml.studio.core.io.model_directory import ModelDirectory

from pathlib import Path

from azureml.studio.modules.ml.score.score_generic_module.score_generic_module import ScoreModelModule

from azureml.designer.serving.dagengine.converter import create_dfd_from_dict

from collections import defaultdict

from azureml.designer.serving.dagengine.utils import decode_nan

from azureml.studio.common.datatable.data_table import DataTable

  
  

model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'trained_model_outputs')

schema_file_path = Path(model_path) / '_schema.json'

with open(schema_file_path) as fp:

    schema_data = json.load(fp)

  
  

def init():

    global model

    model = ModelDirectory.load(model_path).model

  
  

def run(data):

    data = json.loads(data)

    input_entry = defaultdict(list)

    for row in data:

        for key, val in row.items():

            input_entry[key].append(decode_nan(val))

  

    data_frame_directory = create_dfd_from_dict(input_entry, schema_data)

    score_module = ScoreModelModule()

    result, = score_module.run(

        learner=model,

        test_data=DataTable.from_dfd(data_frame_directory),

        append_or_result_only=True)

    return json.dumps({"result": result.data_frame.values.tolist()})
```
- [ ] inference with deployed endpoint in a script (replace the *test deployment* job)
``` python
from azure.ai.ml import MLClient, Input
from azure.ai.ml.entities import BatchEndpoint, BatchDeployment, Model, AmlCompute, Data, BatchRetrySettings
from azure.ai.ml.constants import AssetTypes, BatchDeploymentOutputAction
from azure.identity import DefaultAzureCredential

subscription_id = "<subscription>"
resource_group = "<resource-group>"
workspace = "<workspace>"

ml_client = MLClient(DefaultAzureCredential(), subscription_id, resource_group, workspace)

job = ml_client.batch_endpoints.invoke( endpoint_name=endpoint_name, inputs=Input(path="https://pipelinedata.blob.core.windows.net/sampledata/mnist", type=AssetTypes.URI_FOLDER) )
```
- [ ] embed a python script in pipeline`

- [ ] copyright issues discussion
- [ ] 