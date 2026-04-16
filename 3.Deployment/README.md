# The deployment notebook

The main purpose of this section is to build our `Hugging Face Model`

## 1. What is Hugging Face Model ?

In the AWS ecosystem, the Hugging Face Model is a high-level Python class provided by the SageMaker Python SDK that defines a deployable machine learning artifact. 

While the Estimator is a blueprint for a job(training) that starts and then stops, the Model is a blueprint for a permanent (or serverless) Endpoint that will live in the cloud to answer real-time predictions.

It acts as a bridge that connects your trained weights stored on S3 to a live, managed Docker environment specifically optimized for Hugging Face Transformers.

## 2. Prepare our Hugging Face Model
This is implemented in the [Deployment.ipynb](https://github.com/egafossojm/ml-text-classification-project/blob/main/3.Deployment/Deployment.ipynb) notebook.

### a. Make sure a supported transformers library is installed
- View the current version
```python
import transformers
print(transformers.__version__)
```
- If a specific version is needed
```python
%pip install transformers==4.30.2
```

### b. Create the HuggingFace Model
```python
from sagemaker.huggingface import HuggingFaceModel
from sagemaker import get_execution_role

role = get_execution_role()

model_s3_path = 's3-uri-to-your-model'

huggingface_model = HuggingFaceModel(

    model_data = model_s3_path,
    role = role,
    transformers_version = "4.6",
    pytorch_version = "1.7",
    py_version = "py36",
    entry_point = "inference.py"
    
)
```

## 3. Understanding the inference script: `inference.py`
In order to better understand the inference script `inference.py`, consult the following 2 documents:

- [LLM-Transformers](https://github.com/egafossojm/ai-engineering/blob/main/math-llm-transformers/understand-llm-and-transformers.md)
- [ExperimentatationNotebook](https://github.com/egafossojm/ml-text-classification-project/blob/main/2.Training/ExperimentationNotebook.ipynb)


## 4, Deployment

-  Deploy the model

```python
predictor = huggingface_model.deploy(
    initial_instance_count = 1,
    instance_type = "ml.m5.xlarge",
    endpoint_name = "multiclass-text-classification-endpointv1"
)
```
This will create endpoint model in AWS.

The endpoint creation will get started once you will execute this cel.

In Sagemaker console you can view the deployment status by going to `Amazone SageMaker AI > Endpoints > Select the endpoint`.

In the notebook the output will show an `!` and the end when it is done. (`----!`)

> [!NOTE]
>
> If you do not use the endpoint for inference, you can go to the console in `Amazone SageMaker AI > Endpoints` and delete the deployed endpoint in order to stop charge.
>
> You can go back to the notebook at anytime to run this cel again(Make sure you change the endppoint name, since it's uniq).

- Test Prediction

Once the model deployment is done, provide some inputs data(in the correct format) to the model predict() function.

```python
data = {"inputs": "The stock market hit an all time low"}
prediction = predictor.predict(data)
```

Run this cel to see the result.



