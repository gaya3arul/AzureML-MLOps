# Operationalizing Machine Learning with AutoML & AML Pipelines

## Table of contents
   * [Overview](#Overview)
   * [Architectural Diagram](#Architectural-Diagram)
   * [Key Steps](#Key-Steps)
   * [Screenshots](#Screenshots)
   * [Screen Recording](#Screen-Recording)
   * [Comments and future improvements](#Comments-and-future-improvements)
   * [Dataset Citation](#Dataset-Citation)
   * [References](#References)


#Overview

In this project, Azure Machine Learning is used to build and operationalize a classification model that 
utilizes the UCI Bank Marketing dataset (https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) to predict if a bank client will subscribe to a term deposit or not. The focus in this project will be on the operationalization of the best model that
results from an AutoML training run. 




   

## Architectural Diagram
![architectur_diagram](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/architectur-diagram.png) 

## Key Steps
**Step 1: Authentication**

I have skipped this step as I am notauthorized to create a security principal.


**Step 2: Automated ML Experiment**

The dataset has been downloaded from https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv and registered as a dataset in the Azure Machine Learning workspace with all the instructions given in the project section.

Registered dataset:
![registered_dataset](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/Registered_dataset.png)
I created an AutoML run with the registered Bank marketing dataset. Classification has been selected and the "Explain best model" option has been checked. The "Exit criterion" has been reduced to 1 hour and the "Concurrency" has been set to 5.

Completed experiment:
![automl_run](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/automl-run.png)

Best model:
![best_model](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model.png)


**Step 3: Model Deployment**

The best model has been selected for deployment using Azure Container Instance (ACI). Authentication has been enabled. This has been completed using the Azure Machine Learning studio (the GUI).

Model is deployed and it is in healthy state:

![deploy_model](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/deploy-model.png)

**Step 4: Enabling Logging**

Application Insights has been enabled for the deployed model using the Python SDK and logs have been retrieved by running the logs.py script.

Enabling Application Insights:
![application_insights_enabled](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/application-insights-enabled.png)

Retrieving Application Insights Logs:
![application_insights_logs](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/application_insights_logs.png)


**Step 5: Swagger Documentation**

The Swagger JSON file has been downloaded from the deployed model and copied to the swagger directory. The swagger.sh script has been run to download the latest Swagger container and run it on port 9080. Finally, the serve.py script has been run to start a Python server on port 8000 to serve the Swagger JSON file.

Swagger running on localhost showing the HTTP API methods and responses for the model:
![swagger_ui](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/swagger-ui.png)


**Step 6: Consuming Model Endpoint**

The endpoint.py script has been run to interact with the trained model. For this the scoring_uri and key have been modified to match the deployed service.

Running the endpoint.py script against the API producing JSON output from the model:
![consuming-model-endpoint](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/consuming-model-endpoint.png)


**Step 7: Creating and Publishing a Pipeline**

The aml-exercise-pipelines-with-automated-machine-learning-step.ipynb notebook has been modified and run to create, publish and consume a pipeline.

AML studio pipeline section showing the pipeline REST endpoint with a status of ACTIVE:
![pipeline_endpoints](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/pipeline-endpoint.png)

The bankmarketing dataset with the AutoML module:
![pipeline_run](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/pipeline-run.png)

The "Use RunDetails Widgets" in the Jupyter Notebook showing the step runs:
![RunWidgets](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/RunWidgets.png)

![automl-module-finished](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/automl-module-finished.png)

A completed pipeline run:
![pipeline-runs](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/pipeline-runs.png)

Benchmarking :(optional step)
Ran shell script benchmark.sh. It takes around 144 ms per request and there are no failed requests.

![Benchmarking](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/benchmark.png)
**Step 8: Documentation**

A screencast showing the entire process of the working ML application has been created. The link can be found in the next section. In addition, this README has been created which contains an overview of the project, an architectural diagram, a short description of the project main steps as well as a short section on how to improve the project in the future.

## Screen Recording
The screencast containing the project results has been recorded using the Camtasia and has been uploaded to YouTube: https://youtu.be/VWn0fYbOMiE.

## Improvement Suggestions
- Dataset: The registered dataset can directly point to the URI instead of making use of a local file upload so that when the data gets updated, automatically the new data will be used within the pipeline run.
- Model: An even better model could be developed by making use of feature engineering or hyperparameter tuning using Hyperdrive.Exporting the model using ONNX. 
-  Pipeline: The pipeline could be scheduled to run on a regular basis or execute in a trigger-based manner .Using Parallel run in a pipeline when large dataset is used as input. 
