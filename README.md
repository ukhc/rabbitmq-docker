# RabbitMQ for Docker and Kubernetes

## Reference
- https://hub.docker.com/_/rabbitmq

## Docker deployment to the local workstation

~~~
# start the container
docker run -d --name rabbitmq rabbitmq:3.7.15

# see the status
docker container ls

# destroy the container
docker container stop rabbitmq
docker container rm rabbitmq
~~~


## Kubernetes deployment to the local workstation (macOS only)

## Prep your local workstation (macOS only)
1. Clone this repo and work in it's root directory
1. Install Docker Desktop for Mac (https://www.docker.com/products/docker-desktop)
1. In Docker Desktop > Preferences > Kubernetes, check 'Enable Kubernetes'
1. Click on the Docker item in the Menu Bar. Mouse to the 'Kubernetes' menu item and ensure that 'docker-for-desktop' is selected.

Deploy (run these commands from the root folder of this repo)
~~~
kubectl apply -f ./kubernetes/rabbitmq.yaml
~~~

Delete
~~~
kubectl delete -f ./kubernetes/rabbitmq.yaml
~~~





## Deploy RabbitMQ on Kubernetes with the Kubernetes Peer Discovery Plugin

This is a RabbitMQ cluster deployment on Kubernetes with peer discovery via `rabbitmq-peer-discovery-k8s` plugin.


## Usage

* Deploy `YAML` file to local Kubernetes:

```
kubectl apply -f kubernetes-deployment.yaml
```
6. Check the cluster status:

Wait few seconds....then run

```
kubectl exec --namespace=queue rabbitmq-0 rabbitmqctl cluster_status
```

The output should look something like this:

```
Cluster status of node 'rabbit@172.17.0.2'
[{nodes,[{disc,['rabbit@172.17.0.2','rabbit@172.17.0.4',
                'rabbit@172.17.0.5']}]},
 {running_nodes,['rabbit@172.17.0.5','rabbit@172.17.0.4','rabbit@172.17.0.2']},
 {cluster_name,<<"rabbit@rabbitmq-0.rabbitmq.test-rabbitmq.svc.cluster.local">>},
 {partitions,[]},
 {alarms,[{'rabbit@172.17.0.5',[]},
          {'rabbit@172.17.0.4',[]},
          {'rabbit@172.17.0.2',[]}]}]
```

* Ports:
	* `http://localhost:31672`: [HTTP API and management UI](https://www.rabbitmq.com/management.html)  login with guest:guest
	* `amqp://guest:guest@localhost:30672`: [AMQP 0-9-1 and AMQP 1.0](https://www.rabbitmq.com/networking.html#selinux-ports)

* Scaling the number of nodes:

```
kubectl scale statefulset/rabbitmq --replicas=5 --namespace=rabbitmq
```
 
