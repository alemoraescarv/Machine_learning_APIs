```python
import os
import fairing
```


```python
# Setting up google container repositories (GCR) for storing output containers
# You can use any docker container registry istead of GCR
GCP_PROJECT = fairing.cloud.gcp.guess_project_name()
DOCKER_REGISTRY = 'gcr.io/{}/fairing-job'.format(GCP_PROJECT)
fairing.config.set_builder('append',base_image='tensorflow/tensorflow:latest-py3', registry=DOCKER_REGISTRY, push=True)
fairing.config.set_deployer('job')
```


```python
def train():
    print("hello world!")
```


```python
# Local training
train()
```


```python
remote_train = fairing.config.fn(train)
remote_train()
```
