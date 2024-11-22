# Installation Instructions:
## Install Docker:
```
sudo apt install docker.io
```
## Install kubectl:
```
sudo snap install kubectl --classic
kubectl version --client
Client Version: version.Info{Major:"1", Minor:"26", GitVersion:"v1.26.0", ...}
```

## Install kind:
```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

## Create a kind cluster:

```
kind create cluster
```

## Create a Configuration File for the `Multi-Node` Cluster
- Create a `kind.yaml` file:
- You can find the kind configuration file here: [kind.yaml](kind.yaml).
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker

```
- Create the Cluster
```
kind create cluster --config kind.yaml
```
- Verify the nodes:
```
kubectl get nodes
```
- Delete the Cluster
```
kind delete cluster
```
### This will remove the cluster and free up the resources on your system.


