# What is Kubernetes?

Primarily it is an API. k8s doesn't really "exist"

It is also a cli tool (kubectl) which speaks to the API.

Behind the scenes there are many things which implement that API.

# Setup

## Microk8s
```
#
# Setup microk8s
#
snap install --classic microk8s

# Optional (but recommended)
snap install kubectl
apt install kubectx # <- Provides kubens and other tools

microk8s enable metallb ingress hostpath-storage

# when metallb prompts for an IP range give it some RFC1918 range like 
# 192.168.5.1-192.168.5.100). This will be a range it uses to give IPs to 
# load balancers

# Create a configuration for the standard kubectl tool to connect to microk8s
microk8s config > ~/.kube/config # create .kube if it doesn't exist

```
# Create a searx deployment
```
#
# Create our service
#
kubectl create namespace searx
kubectl apply -f pvc.yml -n searx
kubectl apply -f searx_deployment.yml -n searx
kubectl apply -f searx_ingress.yml -n searx
kubectl apply -f searx_service.yml -n searx
```

The searx ingress will be configured to pass requests to localhost with the 
hostname of "searx.localhost" to your service. Add an entry for this to your 
`hosts` file:
```
# 127.0.0.1  searx.localhost
```
```
