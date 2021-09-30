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
