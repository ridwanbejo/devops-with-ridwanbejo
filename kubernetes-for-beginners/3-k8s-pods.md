# K8s Commands Quickstart - Pods

> I'm using Ubuntu OS when running these commands.

# A. Prerequisites

Check prerequisites from:

1. 1-local-k8s-with-kind.md
2. 2-docker-quickstart.md

# B. Imperative command examples

```
kubectl run nginx --image=nginx
```

```
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
```

```
kubectl create -f redis-definition.yaml
```

```
kubectl apply -f redis-definition.yaml
```


# C. YAML-based deployment

work in progress