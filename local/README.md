# Local setup

Local testing of the deployment can be done with Minikube. Also see [Notes](Notes.md) on setup and possible issues.


## Startup

To get Minikube running execute the following command:

```
minikube start
```

Then to check and make sure you have the proper context up and running do a

```
kubectl config current-context
```

It should show *minikube*

## Local Namespace

To create a namespace based on file you can use this:

```
kubectl apply -f local/namespace.yml
```

Then this namespace can be used instead of default to launch your pods into.

## Local Ingress Controller

Locally you can run an Ingress Nginx as well, but in a slightly different way

https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/

#### Enable Minikube Nginx Ingres 
To enable the NGINX Ingress controller, run the following command:

`minikube addons enable ingress`

Verify that the NGINX Ingress controller is running:

`kubectl get pods -n kube-system`


## Ingress Resource 

https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/#create-an-ingress-resource

To create a resource for your Ingress Nginx Controller to send traffic to your Service we need to set this up.

```
kubectl apply -f local/services/ingress.yml
```

Once this is up and running you can check for address and port with 

```
kubectl get ingress
```


**NB** We added a host in this file called `smart48k8.test` and you do need to check `minikube ip` to attach this host to the ip in `/etc/host for it to load.

## Local Persistent Volume

to use the storage for local testing apply the one in local directory 

```
kubectl apply -f local/storage/pv.yml
kubectl apply -f local/storage/pvc.yml
```

**NB** Not sure if we need the first one as well here.

and to check it has been created and is running we can use `kubectl get pv` and to delete all (dangerous) use `kubectl delete pvc --all`

*Notes on Persistent volume and testing still needed*

You also need to add persistent storage for the database containers so use

```
kubectl apply -f local/storage/mysql-pv-claim.yml
kubectl apply -f local/storage/redis-pv-claim.yml
```

## Local Deployments 

Local deployments are split in deployments for the app and other containers

To fire up the app with the Laravel and Nginx container run

```
kubectl apply -f local/deployments/app.yml
```

then we have the other deployments excluding the databases:

```
kubectl apply -f local/deployments/horizon.yml
kubectl apply -f local/deployments/php-worker.yml
kubectl apply -f local/deployments/workspace.yml
```
### Databases

To run the MySQL database and Redis containers run

```
kubectl apply -f local/deployments/mysql.yml
kubectl apply -f local/deployments/redis.yml
```