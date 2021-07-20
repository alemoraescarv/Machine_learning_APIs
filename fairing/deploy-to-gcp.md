# Train Locally and Deploy to GCP
The following notebook trains an XGBoost model locally and deploys the resulting model file to GCP using the ML Engine Online Prediction API.


```python
# Copyright 2019 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Update the following variables for your project
PROJECT_ID   = '<project-id>'
VERSION_DIR  = 'gs://<bucket-name>/<folder-name>/'
MODEL_NAME   = '<model-name>'
VERSION_NAME = '<version-name>'
```

### Define training and evaluation functions


```python
import argparse
import pandas as pd
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import Imputer
from xgboost import XGBRegressor
import urllib.request

import logging
logging.getLogger().setLevel(logging.INFO)

TRAINING_URL="https://raw.githubusercontent.com/kubeflow/examples/master/xgboost_ames_housing/ames_dataset/train.csv"
TRAINING_FILE="train.csv"

ESTIMATORS=1000
LEARNING_RATE=0.1
TEST_FRACTION_SIZE=0.25
EARLY_STOPPING_ROUNDS=50

def run_training_and_eval():
    (train_X, train_y), (test_X, test_y) = read_input()
    model = train_model(train_X,
                        train_y,
                        test_X,
                        test_y,
                        ESTIMATORS,
                        LEARNING_RATE)

    eval_model(model, test_X, test_y)

def download(url, file_name):
    with urllib.request.urlopen(url) as response, open(file_name, "wb") as file:
        file.write(response.read())

def read_input(test_size=TEST_FRACTION_SIZE):
    """Read input data and split it into train and test."""
    download(TRAINING_URL, TRAINING_FILE)
    data = pd.read_csv(TRAINING_FILE)
    data.dropna(axis=0, subset=['SalePrice'], inplace=True)

    y = data.SalePrice
    X = data.drop(['SalePrice'], axis=1).select_dtypes(exclude=['object'])

    train_X, test_X, train_y, test_y = train_test_split(X.values,
                                                        y.values,
                                                        test_size=test_size,
                                                        shuffle=False)

    imputer = Imputer()
    train_X = imputer.fit_transform(train_X)
    test_X = imputer.transform(test_X)

    return (train_X, train_y), (test_X, test_y)

def train_model(train_X,
                train_y,
                test_X,
                test_y,
                n_estimators,
                learning_rate):
    """Train the model using XGBRegressor."""
    model = XGBRegressor(n_estimators=n_estimators,
                      learning_rate=learning_rate)

    model.fit(train_X,
              train_y,
              early_stopping_rounds=EARLY_STOPPING_ROUNDS,
              eval_set=[(test_X, test_y)])

    logging.info("Best RMSE on eval: %.2f with %d rounds",
                 model.best_score,
                 model.best_iteration+1)
    return model

def eval_model(model, test_X, test_y):
    """Evaluate the model performance."""
    predictions = model.predict(test_X)
    logging.info("mean_absolute_error=%.2f", mean_absolute_error(predictions, test_y))
```

### Train and evaluate the model locally


```python
(train_X, train_y), (test_X, test_y) = read_input()
model = train_model(train_X,
                        train_y,
                        test_X,
                        test_y,
                        ESTIMATORS,
                        LEARNING_RATE)

eval_model(model, test_X, test_y)
```

### Export the model using the joblib library


```python
import joblib
joblib.dump(model, 'model.joblib')
!gsutil cp model.joblib {VERSION_DIR}
```

### Deploy the model to GCP


```python
from fairing.deployers.gcp.gcpserving import GCPServingDeployer
deployer = GCPServingDeployer()
deployer.deploy(VERSION_DIR, MODEL_NAME, VERSION_NAME)
```

### Send a prediction to the deployed model


```python
from googleapiclient import discovery
ml = discovery.build('ml', 'v1')

resource_name = 'projects/{}/models/{}/versions/{}'.format(PROJECT_ID, MODEL_NAME, VERSION_NAME)
ml.projects().predict(
    name=resource_name,
    body={
        'instances': [
            [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37]
        ]
    }
).execute()
```
