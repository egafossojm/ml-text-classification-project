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
- Add inside the S3 bucket, a other folder called **output**. We are going to store the traning output here in the future.
- Upload our dataset file (**newsCorpora.csv**) into the s3 bucket folder **training-data**.

### 3. Exploratory Data Analysis (EDA)
> [!NOTE]
> All this section is implemented into the [EDA_MultiClassTextClassification.ipynb](https://github.com/egafossojm/ml-text-classification-project/blob/main/1.Dataset/EDA_MultiClassTextClassification.ipynb) notebook.

#### a. Impport the dataset from S3
```python
import pandas as pd

s3_path = 's3-uri-to-your-csv'
df = pd.read_csv(s3_path, sep='\t',names=['ID','TITLE','URL','PUBLISHER','CATEGORY','STORY','HOSTNAME','TIMESTAMP'])
```

#### b. View the dataset to get more infos
```python
df.head()
df.describe()
df.info()
```

#### c. Data processing
Here we will get a subset of our dataset in order to make some transformations and extract the relevant parts for our case.

- Extract TITLE and CATEGORY from the dataset.
```python
df_work = df.copy()
df_work=df_work[['TITLE','CATEGORY']]
```

- Replace letter values in CATEGORY by full name (explained in the dataset description on the web page).
```python
my_dict = {
    'e':'Entertainment',
    'b':'Business',
    't':'Science',
    'm':'Health'
}

def update_category(x):
    return my_dict[x]

df_work['CATEGORY'] = df_work['CATEGORY'].apply(lambda x: update_category(x))
```

- Verify by getting some random TITLES by CATEGORY
```python
import random

def get_random_title_by_category(category):
    filtered_df = df_work[df_work['CATEGORY']==category]
    
    return filtered_df['TITLE'].sample().values[0]


category = 'Entertainment'

random_title = get_random_title_by_category(category)

print(random_title)
```

#### d. Data Visualization
Here we will how is the dataset is distributed

- Bar chart
```python
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))
sns.countplot(data=df_work,x='CATEGORY',order=df_work['CATEGORY'].value_counts().index)
plt.title('Distribution of Categories')
plt.xticks(rotation=45)
plt.show()
```

- Proportions
```python
category_counts = df_work['CATEGORY'].value_counts()
plt.figure(figsize=(8,8))
plt.pie(category_counts,labels=category_counts.index,autopct='%1.1f%%',startangle=140)
plt.title('Proportion of Each Category')
plt.show()
```

