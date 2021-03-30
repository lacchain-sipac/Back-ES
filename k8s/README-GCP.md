# ELK-KIBANA-LOGSTASH

## Deployment Steps

* Authentication:

```shell
#Setup configuration proyect
gcloud init

#Connect to cluster
export CLUSTER_NAME=cluster-hondu-pf
export CLUSTER_ZONE=us-east1-b
export CLUSTER_PROYECT=hondu-pf

gcloud container clusters get-credentials $CLUSTER_NAME --zone $CLUSTER_ZONE --project $CLUSTER_PROYECT
```

* Deploying ELK
  * Deploying elk
  
  ```shell
  kubectl apply -f https://download.elastic.co/downloads/eck/1.2.0/all-in-one.yaml #Downloads the operator for k8s
  kubectl apply -f elk.yml
  ```

  * Getting the elk password
  
  ```shell
  PASSWORD=$(kubectl get secret es-cluster-es-elastic-user -n kube-honduras -o go-template='{{.data.elastic | base64decode}}')
  ```

* Deploy logstash:

```shell
cd logstash
export BUILD_VERSION=6.4.1 #change the version accordingly
export DOCKER_NAME=hondu-pf/logstash
export DOCKER_REGISTRY=crprocesosfiduciarios001.azurecr.io
export DOCKER_IMAGE_TAG="$DOCKER_REGISTRY/$DOCKER_NAME:$BUILD_VERSION"
docker build -t $DOCKER_IMAGE_TAG .
docker push $DOCKER_IMAGE_TAG
cat logstash-deployment.yaml | sed  's|{{DOCKER_IMAGE_TAG}}|'$DOCKER_IMAGE_TAG'|g' | kubectl apply -f -
```

* Deploy Kibana:

```shell
kubectl apply -f kibana-deployment.yaml
```

* Accessing Kibana from the browser:
To access the kibana dashboard go to https://sipac.kaytrust.id/dashboard and use the following:

```shell
user: elastic
password: Use the password obtained lines above.
```
