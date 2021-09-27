# devops-project-4
Deploying Consul service mech on kubernetes cluster, and enabling observability through Prometheus and Grafana.

> **Note:** this project builds on top of [devops-project-3](https://github.com/ansnoussi/devops-project-3) to add a service mesh to the previous architecture.

In addition to securing inter-service communication, the Envoy proxy injected by Consul service mesh will also collect and expose data about the service instance to Prometheus that will be later displayed in a beautiful Grafana dashboard.
