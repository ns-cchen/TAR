Set up environment

1. start colima
```
colima start --arch aarch64 --vm-type=vz --vz-rosetta --network-address
```

2. create cluster
```
k3d cluster create --agents 6
export KUBECONFIG=~/.kube/config
```

3. check nodes
```
kubectl get nodes -o json | jq '.items[] | {name: .metadata.name, hostname: .metadata.labels."kubernetes.io/hostname", zone: .metadata.labels."topology.kubernetes.io/zone", region: .metadata.labels."topology.kubernetes.io/region"}'
```

4. label nodes
```
kubectl label node k3d-k3s-default-agent-0 topology.kubernetes.io/zone=roc-a
kubectl label node k3d-k3s-default-agent-1 topology.kubernetes.io/zone=roc-a
kubectl label node k3d-k3s-default-agent-2 topology.kubernetes.io/zone=roc-a
kubectl label node k3d-k3s-default-agent-3 topology.kubernetes.io/zone=roc-b
kubectl label node k3d-k3s-default-agent-4 topology.kubernetes.io/zone=roc-b
kubectl label node k3d-k3s-default-agent-5 topology.kubernetes.io/zone=roc-b
```

5. Apply configs
```
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f curl.yaml
```

Please refer to this article https://itnext.io/controlling-kubernetes-traffic-with-topology-aware-routing-9b1d51a43bd7