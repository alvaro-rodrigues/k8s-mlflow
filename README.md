# MLFlow on Kubernetes


## About the project

This is a use case with deployment configs for setting up MLFlow on Kubernetes, with a example of model training and model serving inside the Kubernetes cluster.
It was developed in a OpenShift Sandbox, but the configs should work on any Kubernetes distribution.

### Built With

* [MLFlow](https://mlflow.org/)
* [MinIO](https://min.io/)
* [PostgreSQL](https://www.postgresql.org/)
* [FastAPI](https://fastapi.tiangolo.com/)

## Usage

Following some instructions for Openshift (kubectl should work too).
You will have to set you cluster IPs on the config files.

1. Deploy PostgreSQL
```sh
$ oc create -f server-deploy/postgre.yml
```
2. Deploy MinIO
```sh
$ oc create -f server-deploy/minio.yml
```
3. Deploy MLFlow
```sh
$ oc create -f server-deploy/mlflow.yml
```
4. Expose MinIO
```sh
$ oc expose service/mlflow-minio-service
```
5. Login on MinIO and create a bucket called `mlflow`.

6. Expose MLFlow
```sh
$ oc expose service/mlflow-service
```
7. Train the model
```sh
$ oc create -f training/pod_training.yml
```
8. Edit the `MLFLOW_RUN_ID` env variable and set your model run.

9. Deploy the model API
```sh
$ oc create -f serving/deploy_serving.yml
```
10. Expose the model API
```sh
$ oc expose service/iris-model-service
```
### Accessing the application


1. Send a POST request to the iris-model-service route containing a JSON like this:

```json
{
  "sepal": {
    "length": 5.5,
    "width": 2.3
  },
  "petal": {
    "length": 4,
    "width": 1.3
  }
}
```
- An example using [curl](https://curl.se//):
```sh
$ curl -X POST -H "Content-Type:application/json" -d '{"sepal":{"length": 5.5,"width": 2.3}, "petal":{"length": 4,"width": 1.3}}' http://iris-model-service-alvaro-rodrigues-dev.apps.sandbox-m2.ll9k.p1.openshiftapps.com/predict
```
- Which returns:
```json
{
    "flower": "Versicolor"
}
```
## Authors
* **Álvaro Rodrigues** -  [alvaro-rodrigues](https://github.com/alvaro-rodrigues)

## License

Distributed under the MIT License. See `LICENSE` for more information.