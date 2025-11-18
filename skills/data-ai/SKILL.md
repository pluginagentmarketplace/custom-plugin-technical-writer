---
name: data-science-ai
description: Master data science, machine learning, and AI. Use this skill for learning about data pipelines, ML algorithms, deep learning, and production ML systems.
---

# Data Science & AI Skill

## Quick Start: ML Fundamentals

### Python Data Science Stack
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
import matplotlib.pyplot as plt

# Load data
df = pd.read_csv('data.csv')

# Exploratory analysis
print(df.head())
print(df.describe())
print(df.isnull().sum())

# Preprocessing
X = df.drop('target', axis=1)
y = df['target']
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X_scaled, y, test_size=0.2, random_state=42
)

# Model training
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)

# Evaluation
score = model.score(X_test, y_test)
print(f"Accuracy: {score}")
```

## Machine Learning Workflow

### 1. Problem Definition
- Supervised vs Unsupervised learning
- Classification vs Regression
- Evaluation metrics
- Business constraints

### 2. Data Collection & Exploration
```python
# Data quality checks
missing_percentage = (df.isnull().sum() / len(df)) * 100
print(missing_percentage)

# Outlier detection
Q1 = df['column'].quantile(0.25)
Q3 = df['column'].quantile(0.75)
IQR = Q3 - Q1
outliers = df[(df['column'] < Q1 - 1.5 * IQR) | (df['column'] > Q3 + 1.5 * IQR)]

# Visualization
import seaborn as sns
sns.heatmap(df.corr(), annot=True)
plt.show()
```

### 3. Data Preprocessing
- Handling missing values
- Encoding categorical variables
- Normalization/standardization
- Feature engineering
- Handling class imbalance

```python
# Categorical encoding
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])

# Feature scaling
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
df['scaled_feature'] = scaler.fit_transform(df[['feature']])
```

### 4. Model Selection & Training
```python
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import GradientBoostingClassifier

# Cross-validation
models = [
    RandomForestClassifier(),
    GradientBoostingClassifier(),
    LogisticRegression()
]

for model in models:
    scores = cross_val_score(model, X_train, y_train, cv=5)
    print(f"{model.__class__.__name__}: {scores.mean():.3f}")
```

### 5. Hyperparameter Tuning
```python
from sklearn.model_selection import GridSearchCV

params = {
    'n_estimators': [50, 100, 200],
    'max_depth': [5, 10, 15],
    'learning_rate': [0.01, 0.1, 1]
}

grid_search = GridSearchCV(
    GradientBoostingClassifier(),
    params,
    cv=5,
    scoring='accuracy'
)

grid_search.fit(X_train, y_train)
print(f"Best params: {grid_search.best_params_}")
```

## Deep Learning with TensorFlow/PyTorch

### TensorFlow Keras
```python
from tensorflow import keras
from tensorflow.keras import layers

# Build sequential model
model = keras.Sequential([
    layers.Dense(128, activation='relu', input_shape=(20,)),
    layers.Dropout(0.2),
    layers.Dense(64, activation='relu'),
    layers.Dropout(0.2),
    layers.Dense(1, activation='sigmoid')
])

model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

history = model.fit(X_train, y_train, epochs=10, batch_size=32)

# Evaluate
loss, accuracy = model.evaluate(X_test, y_test)
print(f"Accuracy: {accuracy}")
```

### PyTorch Example
```python
import torch
import torch.nn as nn

class NeuralNet(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(20, 128)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(128, 64)
        self.fc3 = nn.Linear(64, 1)
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.sigmoid(self.fc3(x))
        return x

model = NeuralNet()
criterion = nn.BCELoss()
optimizer = torch.optim.Adam(model.parameters())
```

## Natural Language Processing (NLP)

### Text Preprocessing
```python
from sklearn.feature_extraction.text import TfidfVectorizer
import nltk
from nltk.corpus import stopwords

# Tokenization and stopword removal
text = "The quick brown fox jumps over the lazy dog"
tokens = text.lower().split()
stop_words = set(stopwords.words('english'))
filtered = [t for t in tokens if t not in stop_words]

# TF-IDF vectorization
vectorizer = TfidfVectorizer(max_features=1000)
X = vectorizer.fit_transform(documents)
```

### Transformer Models
```python
from transformers import pipeline

# Sentiment analysis
sentiment_pipeline = pipeline("sentiment-analysis")
result = sentiment_pipeline("This is amazing!")

# Named Entity Recognition
ner_pipeline = pipeline("ner")
entities = ner_pipeline("Apple is founded by Steve Jobs")

# Text generation
generator = pipeline("text-generation", model="gpt2")
generated = generator("Once upon a time", max_length=50)
```

## Computer Vision

### Image Classification
```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from tensorflow.keras.applications import MobileNetV2

# Data augmentation
train_datagen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=20,
    width_shift_range=0.2,
    height_shift_range=0.2,
    horizontal_flip=True
)

# Transfer learning
base_model = MobileNetV2(input_shape=(224, 224, 3), include_top=False)
base_model.trainable = False

# Add custom top layers
model = keras.Sequential([
    base_model,
    layers.GlobalAveragePooling2D(),
    layers.Dense(256, activation='relu'),
    layers.Dropout(0.5),
    layers.Dense(num_classes, activation='softmax')
])
```

## Evaluation Metrics

### Classification
```python
from sklearn.metrics import confusion_matrix, classification_report, roc_auc_score

y_pred = model.predict(X_test)

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)

# Precision, Recall, F1-Score
print(classification_report(y_test, y_pred))

# ROC-AUC
auc = roc_auc_score(y_test, y_pred_proba)
```

### Regression
```python
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error

# MAE: Average absolute difference
mae = mean_absolute_error(y_test, y_pred)

# RMSE: Square root of mean squared error
rmse = np.sqrt(mean_squared_error(y_test, y_pred))

# R²: Proportion of variance explained
r2 = r2_score(y_test, y_pred)
```

## Data Engineering Pipelines

### Apache Spark
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("DataPipeline").getOrCreate()

# Read data
df = spark.read.csv('data.csv', header=True, inferSchema=True)

# Transform
df_transformed = df.filter(df.age > 18) \
    .groupBy('category') \
    .agg({'salary': 'avg'}) \
    .orderBy('avg(salary)', ascending=False)

# Write
df_transformed.write.parquet('output_data')
```

## MLOps & Production ML

### Model Serving
```python
from flask import Flask, request
import pickle

app = Flask(__name__)
model = pickle.load(open('model.pkl', 'rb'))

@app.route('/predict', methods=['POST'])
def predict():
    data = request.json
    features = [data['feature1'], data['feature2']]
    prediction = model.predict([features])[0]
    return {'prediction': float(prediction)}
```

### Model Monitoring
```python
import mlflow

# Log metrics
mlflow.log_metric("accuracy", 0.95)
mlflow.log_metric("f1_score", 0.92)

# Log model
mlflow.sklearn.log_model(model, "model")

# Load and predict
loaded_model = mlflow.sklearn.load_model("runs:/model")
predictions = loaded_model.predict(X_test)
```

## Best Practices

✅ **DO:**
- Understand your data first
- Use cross-validation
- Split data properly (train/val/test)
- Document experiments
- Track hyperparameters
- Monitor model performance
- Validate assumptions
- Use version control for data and models
- Implement proper error handling
- Consider fairness and bias

❌ **DON'T:**
- Skip exploratory analysis
- Leak test data to training
- Use only accuracy for imbalanced data
- Ignore feature scaling
- Overfit to training data
- Deploy models without testing
- Neglect data quality issues
- Forget to handle missing values
- Use inappropriate metrics
- Ignore production considerations

## Resources

- Scikit-learn documentation
- TensorFlow & Keras
- PyTorch tutorials
- Kaggle competitions
- Andrew Ng's ML courses
- "Hands-On Machine Learning" - Aurélien Géron

## Next Steps
1. Master data analysis with pandas
2. Implement ML algorithms from scratch
3. Learn deep learning fundamentals
4. Build end-to-end ML projects
5. Deploy models to production
