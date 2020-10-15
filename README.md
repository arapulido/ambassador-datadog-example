# ambassador-datadog-example
An example application that shows how ambassador and datadog work together in an Openshift environment

## Setup
This assumes you have Helm 3 CLI installed in your system

### Deploying Ambassador

```
oc apply -f ambassador-deployment/
```

### Deploying Datadog

This will deploy Datadog with APM and Logging enabled and with an Openshift SCC:

```
export DD_API_KEY=<YOUR_DATADOG_API_KEY>
helm install datadog --set datadog.apiKey=$DD_API_KEY datadog/datadog -f datadog-helm-values.yaml
```

### Deploying the Ecommerce app

This will deploy the example Ecommerce application and will create an Ambassador Mapping for its frontend service:

```
oc apply -f ecommerce-app/v1
```

### Setting up Datadog-Ambassador integration

This will enable JSON logging for Envoy and will enable Ambassador's Datadog integration to send traces to Datadog:

```
oc apply -f ambassador-datadog
```

### Restarting the ambassador pod

The Ambassador pod needs restarting for the Tracing object to take effect:

```
oc scale deployment ambassador --replicas=0 -n ambassador
oc scale deployment ambassador --replicas=1 -n ambassador
```
