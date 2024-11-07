# tictactoe
My first Git repository 
Author-Yuvraj Singh
import pandas as pd
data = pd.read_csv('predictive_maintenance_data.csv')
print(data.head())
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
data = data.dropna()
X = data[['temperature', 'vibration', 'pressure']]
y = data['failure']

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(64, input_dim=X_train.shape[1], activation='relu'))
model.add(Dense(32, activation='relu'))
model.add(Dense(32, activation='relu'))
model.add(Dense(16, activation='relu'))
model.add(Dense(1, activation='sigmoid'))

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

history = model.fit(X_train, y_train, epochs=100, batch_size=10, validation_split=0.2)

loss, accuracy = model.evaluate(X_test, y_test)
print(f'Test Accuracy: {accuracy:.2f}')

predictions = model.predict(X_test)

predictions = (predictions > 0.5).astype(int)

import numpy as np
from sklearn.metrics import classification_report
print(classification_report(y_test, predictions))

