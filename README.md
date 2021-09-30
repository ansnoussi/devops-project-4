# devops-project-4
Deploying Consul service mech on kubernetes cluster, and enabling observability through Prometheus and Grafana.

> **Note:** this project builds on top of [devops-project-3](https://github.com/ansnoussi/devops-project-3) to add a service mesh to the previous architecture.

In addition to securing inter-service communication, the Envoy proxy injected by Consul service mesh will also collect and expose data about the service instance to Prometheus that will be later displayed in a beautiful Grafana dashboard.


## Prerequisites

- Ensure to have a running Kubernetes cluster and the `kubectl` command line configured

- Ensure to have `helm` installed

- Ensure we have the latest helm charts for Prometheus, Grafana and Consul
    ```
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts && \
    helm repo add grafana https://grafana.github.io/helm-charts && \
    helm repo add hashicorp https://helm.releases.hashicorp.com && \
    helm repo update
    ```

## Deploying Consul service mesh
For that we will use the official helm chart along with the values in `helm/consul-values.yaml`.
to deploy we use :
```
helm install -f helm/consul-values.yaml consul hashicorp/consul --version "0.32.0" --wait
```

## Deploying the metrics pipeline
we will use Prometheus and Grafana from their community helm charts, but we dont want consul to inject the envoy proxy side-car, that's why we need to disable this by configuring `podAnnotations` with the `"consul.hashicorp.com/connect-inject": "false"` annotation for each of those applications.
- to deploy Prometheus : 
```
helm install -f helm/prometheus-values.yaml prometheus prometheus-community/prometheus --version "14.0.0" --wait
```
- to deploy grafana : 
```
helm install -f helm/grafana-values.yaml grafana grafana/grafana --version "6.9.1" --wait
```
> **Note:** To expose the Grafana UI outside the cluster : `kubectl port-forward svc/grafana 3000:3000` and use creds (admin:password)

## Deploying a demo application
We will be using HashiCups, an application that emulates an online order app for a coffee shop.
application includes a React front end, a GraphQL API, a REST API and a Postgres database.

- to deploy the app :
```
kubectl apply -f hashicups
```
> **Note:** To expose the app outside the cluster : `kubectl port-forward deploy/frontend 8080:80`
