# Operationalizing Machine Learning with AutoML & AML Pipelines

## Table of contents
   * [Overview](#Overview)
   * [Architectural Diagram](#Architectural-Diagram)
   * [Key Steps](#Key-Steps)
   * [Screen Recording](#Screen-Recording)
   * [Improvement Suggestions](#Improvement-Suggestions)


## Overview

In this project, Azure Machine Learning is used to build and operationalize a classification model that 
utilizes the UCI Bank Marketing dataset (https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) to predict if a bank client will subscribe to a term deposit or not. The focus in this project will be on the operationalization of the best model that
results from an AutoML training run. 

## Architectural Diagram
![Architecture-diagram2](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/Architecture-diagram2.png)

This is the process flow that needs to followed

![architectur_diagram](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/architectur-diagram.png) 

## Key Steps
**Step 1: Authentication**

I have skipped this step as I am not authorized to create a security principal.When we use our own Azure Account , this step needs to be done to work on the experiment.


**Step 2: Automated ML Experiment**

The bank marketing dataset from https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv is already registered as a dataset in the Azure Machine Learning workspace .

Registered dataset:
![registered_dataset](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/Registered_dataset.png)

I created an AutoML run with the registered Bank marketing dataset. Classification has been selected and the "Explain best model" option has been checked. The "Exit criterion" has been reduced to 1 hour and the "Concurrency" has been set to 5.

Exploration of the data:
![dataset-explore](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/dataset-explore.png)

Screenshot added :
![classification-model-conf](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/classification-model-conf.png)

Completed experiment: AutoML experiment completed successfully and VotingEnsemble is the best model with 91.98 Accuracy. Explanation of the model is also created.
![automl_run](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/automl-run.png)

Best model:
![best_model](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model.png)

Some best model metrics screenshots below.

![best-model-metrics](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model-metrics.png)
![best-model-precision-ROC](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model-precision-ROC.png)
![best-model-calibration-lift](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model-calibration-lift.png)
![best-model-cumulative-gains.png](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/best-model-cumulative-gains.png)

**Step 3: Model Deployment**

The best model is deployed using Azure Container Instance (ACI). Authentication has been enabled. This has been completed using the Azure Machine Learning studio (the GUI).

Model is deployed and it is in healthy state:

![deploy_model](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/deploy-model.png)

![logs]

**Step 4: Enabling Logging**

Application Insights has been enabled for the deployed model using the Python SDK and logs have been retrieved by running the logs.py script.

Enabling Application Insights:
![application_insights_enabled](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/application-insights-enabled.png)

Retrieving Application Insights Logs:
![logs](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/logs.png)


**Step 5: Swagger Documentation**

The Swagger JSON file has been downloaded from the deployed model and copied to the swagger directory. The swagger.sh script has been run to download the latest Swagger container and run it on port 9080. Finally, the serve.py script has been run to start a Python server on port 8000 to serve the Swagger JSON file.

Swagger running on localhost showing the HTTP API methods and responses for the model:
![swagger-bash](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/swagger-bash.png)

![swagger-bash1](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/swagger-bash1.png)

![swagger_ui](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/swagger-ui.png)

![Swagger-GET-POST](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/Swagger-GET-POST.png)

![http-methods](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/http-methods.png)

![http-responses](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/http-responses.png)


**Step 6: Consuming Model Endpoint**

The endpoint.py script has been run to interact with the trained model. For this the scoring_uri and key have been modified to match the deployed service.

Running the endpoint.py script against the API producing JSON output from the model:
![consuming-model-endpoint](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/consuming-model-endpoint.png)
https://github.com/gaya3arul/operationalize-azure-ml-proj-2/tree/main/Exercise_starter_files

**Step 7: Creating and Publishing a Pipeline**

The aml-exercise-pipelines-with-automated-machine-learning-step.ipynb notebook has been modified and run to create, publish and consume a pipeline.

AML studio pipeline section showing the pipeline REST endpoint with a status of ACTIVE:
![pipeline_endpoints](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/pipeline-endpoint.png)

The bankmarketing dataset with the AutoML module:
![publishe-pipeline-overview](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/publishe-pipeline-overview.png)

The "Use RunDetails Widgets" in the Jupyter Notebook showing the step runs:
![RunWidgets](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/RunWidgets.png)

![automl-module-finished](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/automl-module-finished.png)

A completed pipeline run:
![completed-pipeline-runs](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/completed-pipeline-runs.png)


Benchmarking :(optional step)
Ran shell script benchmark.sh. It takes around 94.86 ms per request and there are no failed requests.

![Benchmarking](https://github.com/gaya3arul/operationalize-azure-ml-proj-2/blob/main/Exercise_starter_files/screenshots/benchmark.png)
**Step 8: Documentation**
Architectural diagram , Screenshots, and Screencast have been used for documentation.

The screencast covers the following. 

The working deployed ML model endpoint
The deployed Pipeline
Available AutoML Model
Successful API requests to the endpoint with a JSON payload

## Screen Recording
The screencast containing the project results has been recorded using the Camtasia and has been uploaded to YouTube: https://youtu.be/eb4cwFwbt_s.

## Improvement Suggestions
- Dataset:The dataset is imbalanced dataset. There are two classes a) people who subscribe to term deposit and b)people who will not. There is one class with less data. We can balance the dataset by generating synthetic data points for the class which has less data points.
- Model: An even better model could be developed by making use of feature engineering or hyperparameter tuning using Hyperdrive.We can further develop the model by assigning more weights to class with lesser data points. Then we need to try to increase difference performance metrics after assigning different weights. The results of the model could be improved.
-  Pipeline: The pipeline could be scheduled to run on a regular basis or execute in a trigger-based manner .in future, we can try parallel run in a pipeline when large dataset is used as input. It will be very useful to learn and apply that knowledge. 
