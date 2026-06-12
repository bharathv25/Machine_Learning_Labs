Problem Statement:
Implementing Basic Machine Learning Model for Intrusion Detection.

STEP-1:
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import classification_report, confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns

pandas
Works with data like an Excel sheet — rows, columns, filtering

numpy
Does mathematical operations on large numbers fast

RandomForestClassifier
The actual ML algorithm — our "20 friends voting"

train_test_split
Splits data into training data and testing data

LabelEncoder
Converts text labels like "normal" "attack" into numbers — ML only understands numbers

classification_report
After prediction, shows how accurate the model is

confusion_matrix
Shows visually how many predictions were correct vs wrong

matplotlib / seaborn
Drawing graphs and charts

STEP-2:

url = "https://raw.githubusercontent.com/defcom17/NSL_KDD/master/KDDTrain+.txt"
We're not uploading any file manually. We're directly downloading the KDD dataset from the internet. That link is the raw dataset file on GitHub.

columns = [
    "duration", "protocol_type", "service", "flag", "src_bytes",
    "dst_bytes", "land", "wrong_fragment", "urgent", "hot",
    "num_failed_logins", "logged_in", "num_compromised", "root_shell",
    "su_attempted", "num_root", "num_file_creations", "num_shells",
    "num_access_files", "num_outbound_cmds", "is_host_login",
    "is_guest_login", "count", "srv_count", "serror_rate",
    "srv_serror_rate", "rerror_rate", "srv_rerror_rate", "same_srv_rate",
    "diff_srv_rate", "srv_diff_host_rate", "dst_host_count",
    "dst_host_srv_count", "dst_host_same_srv_rate",
    "dst_host_diff_srv_rate", "dst_host_same_src_port_rate",
    "dst_host_srv_diff_host_rate", "dst_host_serror_rate",
    "dst_host_srv_serror_rate", "dst_host_rerror_rate",
    "dst_host_srv_rerror_rate", "label", "difficulty"
]

df = pd.read_csv(url, header=None, names=columns)
df.drop("difficulty", axis=1, inplace=True)
print(df.shape)   # rows and columns count
print(df.head())  # first 5 rows

Explanation:
The dataset file has no column headers — just raw numbers and text. So we manually tell pandas what each column is called.
Think of it like a Excel sheet with no header row — you're adding the header yourself.
Some important columns explained simply:
Column
Meaning
duration
How long the connection lasted
protocol_type
TCP, UDP, ICMP — type of connection
src_bytes
How much data was sent
dst_bytes
How much data was received
label
Normal or Attack — this is what we're predicting

#pd.read_csv = read the file as a table
header=None = the file has no header row
names=columns = use our columns list as headers
The dataset has a "difficulty" column we don't need for our model. We delete it.

STEP-3:
print(df['label'].value_counts())
What this tells us:



