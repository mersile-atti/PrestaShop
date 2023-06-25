# PrestaShop
This project aims to deploy a PrestaShop application and a MariaDB database in a local Kubernetes cluster managed with Minikube


This document will guide you through the process of setting up a PrestaShop instance on a local Kubernetes cluster with Minikube.

## Prerequisites

- Minikube and kubectl installed on your local machine
- Docker installed on your local machine

## Step by Step Guide

1. **Start the Minikube cluster**

   Run the following command in your terminal:
    minikube start --nodes=3


2. **Set up the Kubernetes context**

Verify that kubectl is set up to use the Minikube context:
kubectl config get-contexts

The `minikube` context should be the current context. If not, set it:
kubectl config use-context minikube

3. **Create a Namespace**

Create a new namespace `prestashop-project` for the project:
kubectl create namespace prestashop-project

4. **Create a Secret for Environment Variables**

Store environment variables securely using Kubernetes Secrets:
kubectl create secret generic prestashop-env --from-file=prestashop-env.yaml
kubectl create secret generic mariadb-env --from-file=mariadb-env.yaml

5. **Deploy PrestaShop**

Use a Docker image for PrestaShop deployment. Create a `prestashop-deployment.yaml` file and paste the following content:


kubectl apply -f prestashop-deployment.yaml


6. **Create a PersistentVolumeClaim for PrestaShop**

Create a `prestashop-pvc.yaml` file:

kubectl apply -f prestashop-pvc.yaml

7. **Deploy MariaDB**

Similar to PrestaShop, use a Docker image for MariaDB. Create a `mariadb-deployment.yaml` file:

kubectl apply -f mariadb-deployment.yaml

8. **Create a PersistentVolumeClaim for MariaDB**

Create a `mariadb-pvc.yaml` file:

kubectl apply -f mariadb-pvc.yaml

9. **Creating a Service for MariaDB**

Create a `mariadb-service.yaml` file:

kubectl apply -f mariadb-service.yaml

10. **Expose PrestaShop with Ingress**

 In Minikube, an Ingress controller comes preinstalled. If not, install it:
 ```
 minikube addons enable ingress
 ```

 Create a service for PrestaShop by creating a `prestashop-service.yaml` file:

 Apply the service:

 kubectl apply -f prestashop-service.yaml

Then, create an `ingress.yaml` file to expose your PrestaShop service:

kubectl apply -f ingress.yaml
    

11. **Accessing PrestaShop**

First, update your `/etc/hosts` file to route `prestashop.localhost` to your Minikube IP. Run this command to get the IP:

```
minikube ip
```

Now, add the following line to your `/etc/hosts` file:
```
<Minikube_IP> prestashop.localhost
```

Replace `<Minikube_IP>` with your Minikube IP.

Now, open a web browser and navigate to `http://prestashop.localhost` to see your PrestaShop instance.

12. **Clean up**

When you're done with the project, stop the Minikube cluster:

```
minikube stop
```

And to delete the cluster:

```
minikube delete
```