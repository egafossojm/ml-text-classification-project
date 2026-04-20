# ml-text-classification-project
Build, Train and Deploy a Text Classification Model with AWS SageMaker

## Prerequisites
- An AWS account
- Am IAM user with Administrator access
- A Sagemaker domain with a domain user: [Documentation here](https://github.com/egafossojm/ai-engineering/blob/main/amazon-sagemaker/domain-setup.md)

## Project steps

### 1. [Data Exploratory](https://github.com/egafossojm/ml-text-classification-project/tree/main/1.Dataset)

Download, explore, transform and visualize data.

### 2. [Model Training](https://github.com/egafossojm/ml-text-classification-project/tree/main/2.Training)

Prepare the training script and the HuggingFace Estimator, Launch the training Job.

### 3. [Deployment](https://github.com/egafossojm/ml-text-classification-project/tree/main/3.Deployment)

Prepare the inference script, create the HuggingFace Model, Deploy a model Endpoint in SageMaker.

### 4. [Load Testing](https://github.com/egafossojm/ml-text-classification-project/tree/main/4.LoadTesting)

Prepare and launch Load Test in order to get Inference recommendations (Best instance type and configuration)

### 5. [Deploy in production](https://github.com/egafossojm/ml-text-classification-project/tree/main/5.ProductionGradeDeployment)

Make the model accessible

![Architecture](5.ProductionGradeDeployment/img/arch.png)

