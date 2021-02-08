# ambassador-datadog-example
An example application that shows how Ambassador and Datadog work together in an Kubernetes/Openshift environment

## Setup
This assumes you have Helm 3 CLI installed in your system

### Deploying Ambassador

```
kubectl apply -f ambassador-deployment/
```

### Deploying Datadog

#### In Generic Kubernetes
If you are deploying this in a generic Kubernetes cluster, use the following command:

```
export DD_API_KEY=<YOUR_DATADOG_API_KEY>
helm install datadog --set datadog.apiKey=$DD_API_KEY datadog/datadog -f datadog-generic-helm-values.yaml --version=2.8.1
```
This will deploy Datadog with APM and Logging enabled.

#### In Openshift

If you are deploying this in Openshift, use the following command:

```
export DD_API_KEY=<YOUR_DATADOG_API_KEY>
helm install datadog --set datadog.apiKey=$DD_API_KEY datadog/datadog -f datadog-openshift-helm-values.yaml --version=2.8.1
```
This will deploy Datadog with APM and Logging enabled and with an Openshift SCC.

### Deploying the Ecommerce app

This will deploy the example Ecommerce application and will create an Ambassador Mapping for its frontend service:

```
kubectl apply -f ecommerce-app/v1
```

### Setting up Datadog-Ambassador integration

This will enable JSON logging for Envoy and will enable Ambassador's Datadog integration to send traces to Datadog:

```
kubectl apply -f ambassador-datadog
```

### Restarting the ambassador pod

The Ambassador pod needs restarting for the Tracing object to take effect:

```
kubectl scale deployment ambassador --replicas=0 -n ambassador
kubectl scale deployment ambassador --replicas=1 -n ambassador
```

## Replaying traffic

[Go Replay](https://github.com/buger/goreplay) is an open-source network monitoring tool which can record your live traffic, and use it for shadowing, load testing, monitoring and detailed analysis.

This repository has a Go Replay profile available that navigates around the application. To replay the traffic:

* Download the latest release of Go Replay for your platform on [their releases page](https://github.com/buger/goreplay/releases).
* Replay the traffic:

```
./gor --input-file-loop --input-file requests_0.gor --output-http "<AMBASSADOR_URL>"
```
