# tictactoe
My first Git repository 
Author-Yuvraj Singh

import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, accuracy_score, roc_auc_score
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras.callbacks import EarlyStopping
import numpy as np

# Load and preprocess the data
data = pd.read_csv('predictive_maintenance_data.csv')
data = data.dropna()
X = data[['temperature', 'vibration', 'pressure']]
y = data['failure']

# Scale the features
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

# Build the ANN model with a modified architecture
model = Sequential()
model.add(Dense(128, input_dim=X_train.shape[1], activation='selu'))
model.add(Dropout(0.2))  # Dropout for regularization
model.add(Dense(64, activation='selu'))
model.add(Dropout(0.2))
model.add(Dense(32, activation='selu'))
model.add(Dense(16, activation='selu'))
model.add(Dense(1, activation='sigmoid'))  # Sigmoid for binary classification

# Compile the model with additional metrics
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy', 'AUC'])

# Early stopping to prevent overfitting
early_stopping = EarlyStopping(monitor='val_loss', patience=10, restore_best_weights=True)

# Train the model
history = model.fit(X_train, y_train, epochs=150, batch_size=20, validation_split=0.2, callbacks=[early_stopping])

# Evaluate the model
loss, accuracy, auc = model.evaluate(X_test, y_test)
print(f'Test Accuracy: {accuracy:.2f}')
print(f'Test AUC: {auc:.2f}')

# Generate predictions and evaluate further
predictions = (model.predict(X_test) > 0.5).astype(int)
print(classification_report(y_test, predictions))
print(f'Overall Accuracy: {accuracy_score(y_test, predictions):.2f}')
print(f'Overall AUC Score: {roc_auc_score(y_test, predictions):.2f}')

history = model.fit(X_train, y_train, epochs=100, batch_size=10, validation_split=0.2)

loss, accuracy = model.evaluate(X_test, y_test)
print(f'Test Accuracy: {accuracy:.2f}')

predictions = model.predict(X_test)

predictions = (predictions > 0.5).astype(int)

import numpy as np
from sklearn.metrics import classification_report
print(classification_report(y_test, predictions))

