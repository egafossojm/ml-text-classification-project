# Prepare the training notebook

The main purpose of this section is to build our `Hugging Face Estimator`

## 1. What is Hugging Face Estimator ?

In the AWS ecosystem, the Hugging Face Estimator is a high-level Python class provided by the SageMaker Python SDK. It allows you to train Transformer models (for NLP, Computer Vision, or Audio) on fully managed AWS infrastructure with minimal setup.

It is the result of a strategic partnership between AWS and Hugging Face to streamline the MLOps lifecycle.

**What does it actually do?**

Without this estimator, you would have to manually build a Docker container, install specific versions of PyTorch or TensorFlow, and manage the transformers and datasets libraries yourself.\
The Hugging Face Estimator automates this by providing:
- On-demand Infrastructure: It automatically spins up EC2 instances (CPU or GPU) only for the duration of the training.
- Deep Learning Containers (DLCs): It uses pre-built, optimized, and secure Docker images maintained by AWS.
- Managed Training: It handles data downloading from S3, script execution, and uploading the final model artifacts back to S3.

## 2. Prepare our Hugging Face Estimator
This is implemented in the [TrainingNotebook.ipynb](https://github.com/egafossojm/ml-text-classification-project/blob/main/2.Training/TrainingNotebook.ipynb) notebook.

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

### b. Get the Training Notebook IAM Role
```python
from sagemaker import get_execution_role

role = get_execution_role()
```

### c. initiate a sagemaker session
```python
sagemaker_session = sagemaker.Session()
```
It is the authenticated communication bridge that orchestrates the interaction between your local Python code and the AWS Cloud infrastructure to manage data, hardware, and training jobs.

### d. Create the HuggingFace Estimator
```python
from sagemaker.huggingface import HuggingFace

huggingface_estimator = HuggingFace(
                        entry_point = 'script.py',#this will have the information about the model architecture, training data, loss function and etc
                        source_dir = './',
                        role = role,
                        instance_count = 1,
                        instance_type = 'ml.p2.xlarge',
                        transformers_version = '4.6',
                        pytorch_version = '1.8',
                        output_path = 's3-uri-to-where-your-model-output-will-be',
                        py_version = 'py36',
                        hyperparameters = {'epochs':2,
                                            'train_batch_size':4,
                                            'valid_batch_size':2,
                                            'learning_rate':1e-05
                                          },
                        enable_sagemaker_metrics = True)
```

### e. Add the "Launch Button"
```python
huggingface_estimator.fit()
```

> [!WARNING]
> Do not run this if the **script.py** is not complete and ready.

`.fit()` is the "Launch" button that tells AWS to start the trainig.

**What happens behind the scenes?**

The moment you run `.fit()`, the `SageMaker Session` performs these 5 steps automatically:

- **Environment Packaging**: It zips up your source_dir (containing your script.py and any requirements.txt).
- **Infrastructure Provisioning**: It requests the ml.p2.xlarge instance from EC2.
- **Data Transfer**: It downloads your training data from S3 into that new instance.
- **Container Execution**: It pulls the Hugging Face Docker image, injects your code, and runs your training script.
- **Artifact Cleanup**: Once the training is done, it saves the model to your output_path and immediately terminates the instance so you stop paying for it.
