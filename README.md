
# Operationalizing Machine Learning

In this project we use Microsoft Azure to configure a cloud based Machine Learning model. We also explore the model deployment as an HTTP REST API endpoint, Swagger API documentation, Apache benchmarking of the deployed endpoint and consumption of the endpoint using JSON documents as an HTTP POST request. Finally we see how to create, publish and consume a pipeline using Azure SDK.

The dataset used for this project is the [Bank Marketing](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing) Dataset. 
This data is related with direct marketing campaigns of a Portuguese banking institution (based on phone calls) that whether the person will subscribe to the bank term deposit or not. It contains input attributes such as `age`, `job`, `marital status`, `education` etc and output attribute `y` which has values `yes` or `no` which states whether the client has subscribed to a term deposit or not.

## Architectural Diagram

The following diagram shows the overall architecture of the system.

![](images/Architecture.png) 

## Key Steps

### Step 1: Authentication

- In this step we enable authentication as it is crucial for the continuous flow of operations.
Authentication types supported by Azure are Key- based, Token- based and Interactive.

- We will also create a Service Principle (SP) and allow the SP to access the workspace we will be working with. Using a service principal is a great way to allow authentication while reducing the scope of permissions, which enhances security.

*NOTE*: As we are using Azure Account provided by Udacity, we don't need to perform this step.

### Step 2: Automated ML Experiment

In this step, we create an experiment using Automated ML, configure a compute cluster, and use that cluster to run the experiment.

- First we need to upload the Bank Marketing dataset (`bankmarketing_train.csv`) that is provided to Azure Machine Learning Studio.

**Registered Datasets**

![](images/Registered%20Dataset.png)

**Bank Marketing Dataset Uploaded**

![](images/Bank%20Marketing%20Dataset.png)

- Next, we setup a New Automated ML Run by selecting the Dataset we uploaded and creating a new compute cluster. Then we run the Experiment using Classification. 

**Auto ML Run Completed** - The Run takes about 20 minutes to get completed.

![](images/Auto%20ML%20Run.png)

**Auto Ml Run Details**

![](images/Auto%20ML%20Run%20Details.png)

**Auto ML Run Models** - The dataset is run on various algorithms to find the best model that gives the highest Accuracy. After the experiment run completes, a summary of all the models and their metrics are shown, including explanations.

![](images/Auto%20ML%20Run%20Models.png)

**Auto ML Run Best Model** - **Voting Ensemble** was the best model with **91.92%** Accuracy.

![](images/Auto%20ML%20Run%20Best%20Model.png)


### Step 3: Deploy the Best Model - 

In this step we deploy the Best Model using **Azure Container Instance** as it will allow us to interact with the HTTP API service and also interact with the model by sending data over POST requests.

**Deployed Model Details** 

![](images/Deployed%20Model%20Details.png)
![](images/Deployed%20Model%20Details%202.png)

### Step 4: Enable Application Insights -

Now that the Best Model is deployed, we enable Application Insights and retrieve logging output to monitor the health of the deployed service.
To do so, we run the `logs.py` file and this will dynamically authenticate to Azure, enable Application Insights and display the logs for deployed model.

The logs generated after running the script are - 

![](images/Enable%20Application%20Insight%20Terminal.png)

The `Application Insights Enabled` is now set to `true`

![](images/Enable%20Application%20Insights.png)

### Step 5: Swagger Documentation -

In this step, we will consume the deployed model using Swagger to interact with the HTTP REST API endpoint documentation.
For this we will first downlad the `swagger.json` file that Azure provides for deployed models. Next we will run the `swagger.sh` file to pull the latest swagger-ui docker image and run it on port 9000. Finally we run `serve.py` script to serve the `swagger.json` for our model on an HTTP server.

To interact with deployed web service's API resources, we go to localhost:9000 and change URL on Swagger UI to http://localhost:8000/swagger.json as shown below -

![](images/Swagger%20API%20Document.png)

HTTP API GET Operation -

![](images/Swagger%20GET.png)

HTTP API POST Operation -

![](images/Swagger%20POST.png)

HTTP API Response for POST Operation -

![](images/Swagger%20POST%20Response.png)

Models -

![](images/Swagger%20Model%20Default.png)

### Step 6: Consume Model Endpoints and Benchmark

**Consume Model Endpoints**

Now that the model is deployed, use the `endpoint.py` script provided to interact with the trained model. In this step, we run the script, modifying both the `scoring_uri` and the `key` to match the key for our service and the URI that was generated after deployment.

Response received from running the script is as follows -

![](images/Consume%20Endpoint%20Result.png)

**Benchmark**

In this step we benchmark our model using Apache Bench to load-test our model. For this we run the `benchmark.sh` file and receive the following output -

![](images/Bechmark%201.png)

![](images/Benchmark%202.png)

![](images/Benchmark%203.png)

![](images/Benchmark%204.png)

### Step 7: Create, Publish and Consume a Pipeline

For this step, we will use `aml-pipelines-with-automated-machine-learning-step.ipynb` Notebook to create, publish and consume a Pipeline. Publishing a Pipeline will allow us to interact with it over HTTP.

**Pipeline Runs**

![](images/Pipeline%20Runs%20Completed.png)

**Pipeline Endpoints**

![](images/Pipeline%20Endpoints.png)

**`new-experiment` Running**

![](images/new-experiment%20Running.png)

**`pipeline-rest-endpoint` Running**

![](images/pipeline-rest-endpoint%20Running.png)

**`new-experiment` Run Completed**

![](images/new-experiment%20Run%20Completed.png)

**`new-experiment` Run Detail Widget**

![](images/Auto%20ML%20Run%20Detail%20Widget%201.png)

**`pipeline-rest-endpoint` Run Completed**

![](images/pipeline-rest-endpoint%20Run%20Completed.png)

**`pipeline-rest-endpoint` Run Detail Widget**

![](images/AutoML%20Run%20Detail%20Widget%202.png)

**Published Pipeline Overview showing the the REST endpoints and the status `Active`**

![](images/Published%20Pipeline%20Overview.png)

## Screen Recording

Screen recording explaining the steps followed in the project can be viewed using this [Link](https://youtu.be/nQZ4gHd64Wg).

## Future Work 

- Class balancing - The dataset used for training the model was imbalanced with size of smallest class = 3692 out of 32950 training samples. This imbalanced data can lead to a falsely perceived positive effect of a model's accuracy because the input data has bias towards one class. Thus the training data can be made more balanced to remove this bias.

- Using Deep Learning to get better Accuracy.

