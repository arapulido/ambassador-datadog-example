## Getting Started

There are certain dependencies between the steps, so
it's important to follow these steps in this order to
install your configured Ambassador Edge Stack.


## Install the Ambassador Edge Stack

Connect to your cluster and then run the following commands:

  kubectl apply -f 1-aes-crds.yaml && \
  kubectl wait --for condition=established --timeout=90s crd -lproduct=aes && \
  kubectl apply -f 2-aes.yaml && \
  kubectl -n ambassador wait --for condition=available --timeout=90s deploy -lproduct=aes


## Install Your Custom Resources

Now run the following commands to configure your Ambassador deployment with your custom manifests
generated from your configuration:

  kubectl apply -f 3-user.yaml

## Use and share your Kubernetes playground

Navigate to https://$AMBASSADOR_SERVICE_IP/ and start exploring your Kubernetes environment!
