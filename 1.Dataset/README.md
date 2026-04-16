# Dataset

### 1. Download the dataset
- You can download the dataset here:  https://archive.ics.uci.edu/dataset/359/news+aggregator.\
You will get a zip file named **news+aggregator.zip**
- Extract it on your machine. our dataset resides in the **newsCorpora.csv** file.
```bash
$ ls -alh 
total 101M
drwxr-xr-x.  3 user user   88 Apr 16 10:05 .
drwxr-xr-x. 33 user user  48K Apr 16 10:07 ..
-rw-r--r--.  1 user user 3.0M Feb 27  2016 2pageSessions.csv
drwxrwxr-x.  2 user user   78 Feb 28  2016 __MACOSX
-rw-r--r--.  1 user user  98M Feb 27  2016 newsCorpora.csv
-rw-rw-r--.  1 user user 2.6K Feb 28  2016 readme.txt
```


### 2. Upload the dataset to Amazon S3 Bucket

- Create a S3 bucket in the same AWS region as the Amazon SageMaker domain.
- Add inside the S3 bucket, a folder called **training-data**.
- Upload our dataset file (**newsCorpora.csv**) into the s3 bucket folder **training-data**.

### 3. Exploratory Data Analysis (EDA)
> [!NOTE]
> All this section is implemented into the [EDA_MultiClassTextClassification.ipynb](https://github.com/egafossojm/ml-text-classification-project/blob/main/1.Dataset/EDA_MultiClassTextClassification.ipynb) notebook.

