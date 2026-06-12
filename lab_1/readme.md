 Intrusion Detection System using Machine Learning

A beginner-friendly ML project that detects network intrusions (attacks vs. normal traffic) using a Random Forest Classifier trained on the NSL-KDD dataset.

Project Overview

This project builds a binary classification model that can distinguish between:
Normal — legitimate network traffic
Attack— malicious or suspicious activity

The model achieved ~99.97% accuracy on the test set, correctly identifying 13,415 attacks and missing only 7.

 Tech Stack & Libraries

| Library        | Purpose                                          |
|--------------- |--------------------------------------------------|
| `pandas`       | Load and manipulate tabular data (like Excel)    |
| `numpy`        | Fast mathematical operations on arrays           |
| `scikit-learn` | ML model, train/test split, encoding, evaluation |
| `matplotlib`   | Plotting graphs and charts                       |
| `seaborn`      | Drawing the confusion matrix heatmap             |


Dataset

NSL-KDD Dataset — a benchmark dataset for network intrusion detection.

- Source: [GitHub – defcom17/NSL_KDD](https://raw.githubusercontent.com/defcom17/NSL_KDD/master/KDDTrain+.txt)
- Loaded directly from URL — no manual download needed
- 41 features + 1 label column (42 total; `difficulty` column dropped)

Key Features

| Column          | Meaning                           |
|---------------- |---------------------------------- |
| `duration`      | How long the connection lasted    |
| `protocol_type` | TCP, UDP, or ICMP                 |
| `src_bytes`     | Data sent by source               |
| `dst_bytes`     | Data received by destination      |
| `label`         | **Target** — `normal` or `attack` |

Workflow

Step 1 — Import Libraries
python
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import classification_report, confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns

Step 2 — Load Dataset
python
url = "https://raw.githubusercontent.com/defcom17/NSL_KDD/master/KDDTrain+.txt"

columns = [
    "duration", "protocol_type", "service", "flag", "src_bytes", "dst_bytes",
    "land", "wrong_fragment", "urgent", "hot", "num_failed_logins", "logged_in",
    "num_compromised", "root_shell", "su_attempted", "num_root", "num_file_creations",
    "num_shells", "num_access_files", "num_outbound_cmds", "is_host_login",
    "is_guest_login", "count", "srv_count", "serror_rate", "srv_serror_rate",
    "rerror_rate", "srv_rerror_rate", "same_srv_rate", "diff_srv_rate",
    "srv_diff_host_rate", "dst_host_count", "dst_host_srv_count",
    "dst_host_same_srv_rate", "dst_host_diff_srv_rate", "dst_host_same_src_port_rate",
    "dst_host_srv_diff_host_rate", "dst_host_serror_rate", "dst_host_srv_serror_rate",
    "dst_host_rerror_rate", "dst_host_srv_rerror_rate", "label", "difficulty"
]

df = pd.read_csv(url, header=None, names=columns)
df.drop("difficulty", axis=1, inplace=True)

Step 3 — Explore the Data
python
print(df['label'].value_counts())

Step 4 — Simplify Labels (Binary Classification)
python
# Convert all attack types → 'attack', keep 'normal' as-is
df['label'] = df['label'].apply(lambda x: 'normal' if x == 'normal' else 'attack')
print(df['label'].value_counts())

> The original dataset has 21 attack subcategories. We simplify to just two classes: **normal** and **attack**.

### Step 5 — Encode Categorical Columns
python
le = LabelEncoder()
for col in ['protocol_type', 'service', 'flag', 'label']:
    df[col] = le.fit_transform(df[col])

> ML models only understand numbers. `LabelEncoder` converts text values like `tcp`, `udp`, `normal`, `attack` into integers.

Step 6 — Train / Test Split
python
X = df.drop('label', axis=1)
y = df['label']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

print("Training size:", X_train.shape)
print("Testing size:", X_test.shape)


Step 7 — Train the Model
python
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
print("Model training complete!")

> **Random Forest** = 100 decision trees each voting on the result. The majority vote wins — making it robust and accurate.

Step 8 — Evaluate the Model
python
y_pred = rf_model.predict(X_test)
print(classification_report(y_test, y_pred))
```

Step 9 — Confusion Matrix
python
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(6, 4))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues',
            xticklabels=['Normal', 'Attack'],
            yticklabels=['Normal', 'Attack'])
plt.xlabel('Predicted')
plt.ylabel('Actual')
plt.title('Confusion Matrix - Intrusion Detection')
plt.show()
```

---

📊 Results

   Predicted: Normal | Predicted: Attack 
-----------------------------------------------------------------------
|   Actual: Normal   | 11,746 ✅ True Negative | 27 ❌ False Positive |
|   Actual: Attack   | 7 ❌ False Negative | 13,415 ✅ True Positive  |

Term                |  Meaning                                               
-------------------------------------------------------------------------------                                                                 
| ✅ True Negative  | Normal traffic correctly identified as normal          |
| ✅ True Positive  | Attack correctly detected                              |
| ❌ False Positive | Normal traffic wrongly flagged as attack (false alarm) |
| ❌ False Negative | Real attack missed — the most dangerous error          |
 
How to Run
bash
# Install dependencies
pip install pandas numpy scikit-learn matplotlib seaborn

# Run in Google Colab
# Execute each step cell by cell


Concepts Explained Simply

Random Forest— Like asking 100 friends to vote on whether something is an attack. The majority wins.
LabelEncoder — A translator that converts words into numbers so the model can understand them.
train_test_split — 80% of data teaches the model; 20% tests how well it learned.
Confusion Matrix — A scorecard showing correct vs. incorrect predictions in a grid.



