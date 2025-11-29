# Kind command examples

> I'm using Ubuntu OS when running these commands.

# A. Prerequisites

1. Ubuntu / Debian installed machine
2. install docker-ce in ubuntu -> [link](https://docs.docker.com/engine/install/ubuntu/)
3. install kind -> [link](https://kind.sigs.k8s.io/docs/user/quick-start#installing-from-release-binaries)
4. install kubectl

```
sudo snap install kubectl --classic
```

# B. Example commands

1. create single node cluster

```
sudo kind create cluster --name single-node-k8s
```

example output:

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kind create cluster --name single-node-k8s
Creating cluster "single-node-k8s" ...
 ✓ Ensuring node image (kindest/node:v1.34.0) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-single-node-k8s"
You can now use your cluster with:

kubectl cluster-info --context kind-single-node-k8s

Have a nice day! 👋
```

2. accessing the cluster via kubectl

```
sudo kind export kubeconfig --name single-node-k8s
```

example output:

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kind export kubeconfig --name single-node-k8s
Set kubectl context to "kind-single-node-k8s"
```

3. checking nodes within the k8s cluster

```
sudo kubectl get nodes
```

example output:

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kubectl get nodes
NAME                            STATUS   ROLES           AGE     VERSION
single-node-k8s-control-plane   Ready    control-plane   5m11s   v1.34.0
```

4. create new folder

```
mkdir /home/vagrant/kind-cluster-multi-nodes
cd /home/vagrant/kind-cluster-multi-nodes
touch kind.yaml
```

Then copy paste this config into kind.yaml:

```
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

Done. Let's continue to step no. 5 for next action.

5. create multi node cluster

```
sudo kind create cluster --name multi-node-k8s --config kind.yaml
```

example output: 

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kind create cluster --name multi-node-k8s --config kind.yaml
Creating cluster "multi-node-k8s" ...
 ✓ Ensuring node image (kindest/node:v1.34.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-multi-node-k8s"
You can now use your cluster with:

kubectl cluster-info --context kind-multi-node-k8s

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
vagrant@vagrant:~/kind-cluster-multi-nodes$
```

6. accessing the cluster via kubectl

```
sudo kubectl cluster-info --context kind-multi-node-k8s
```

example output:

```
Kubernetes control plane is running at https://127.0.0.1:35117
CoreDNS is running at https://127.0.0.1:35117/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

7. checking nodes within the k8s cluster

```
sudo kubectl get nodes
```

example output:

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
multi-node-k8s-control-plane   Ready    control-plane   80s   v1.34.0
multi-node-k8s-worker          Ready    <none>          71s   v1.34.0
multi-node-k8s-worker2         Ready    <none>          71s   v1.34.0
```


8. checking single node within the k8s cluster

```
sudo kubectl describe node multi-node-k8s-worker
```

example output:

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kubectl describe node multi-node-k8s-worker
Name:               multi-node-k8s-worker
Roles:              <none>
Labels:             beta.kubernetes.io/arch=arm64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=arm64
                    kubernetes.io/hostname=multi-node-k8s-worker
                    kubernetes.io/os=linux
Annotations:        node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 29 Nov 2025 02:06:58 +0000
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  multi-node-k8s-worker
  AcquireTime:     <unset>
  RenewTime:       Sat, 29 Nov 2025 02:24:29 +0000
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Sat, 29 Nov 2025 02:24:29 +0000   Sat, 29 Nov 2025 02:06:58 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Sat, 29 Nov 2025 02:24:29 +0000   Sat, 29 Nov 2025 02:06:58 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Sat, 29 Nov 2025 02:24:29 +0000   Sat, 29 Nov 2025 02:06:58 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Sat, 29 Nov 2025 02:24:29 +0000   Sat, 29 Nov 2025 02:07:13 +0000   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.18.0.5
  Hostname:    multi-node-k8s-worker
Capacity:
  cpu:                5
  ephemeral-storage:  18889392Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  hugepages-32Mi:     0
  hugepages-64Ki:     0
  memory:             3995932Ki
  pods:               110
Allocatable:
  cpu:                5
  ephemeral-storage:  18889392Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  hugepages-32Mi:     0
  hugepages-64Ki:     0
  memory:             3995932Ki
  pods:               110
System Info:
  Machine ID:                 56004b9f5b814aa5ae164cf7ec116630
  System UUID:                900b05a8-b438-4974-a04d-b3c9f4ddd9c2
  Boot ID:                    3c33ea50-fe18-462e-a377-118a86a19ad1
  Kernel Version:             6.8.0-88-generic
  OS Image:                   Debian GNU/Linux 12 (bookworm)
  Operating System:           linux
  Architecture:               arm64
  Container Runtime Version:  containerd://2.1.3
  Kubelet Version:            v1.34.0
  Kube-Proxy Version:
PodCIDR:                      10.244.1.0/24
PodCIDRs:                     10.244.1.0/24
ProviderID:                   kind://docker/multi-node-k8s/multi-node-k8s-worker
Non-terminated Pods:          (2 in total)
  Namespace                   Name                CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                ------------  ----------  ---------------  -------------  ---
  kube-system                 kindnet-qwkcs       100m (2%)     100m (2%)   50Mi (1%)        50Mi (1%)      17m
  kube-system                 kube-proxy-5crkh    0 (0%)        0 (0%)      0 (0%)           0 (0%)         17m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests   Limits
  --------           --------   ------
  cpu                100m (2%)  100m (2%)
  memory             50Mi (1%)  50Mi (1%)
  ephemeral-storage  0 (0%)     0 (0%)
  hugepages-1Gi      0 (0%)     0 (0%)
  hugepages-2Mi      0 (0%)     0 (0%)
  hugepages-32Mi     0 (0%)     0 (0%)
  hugepages-64Ki     0 (0%)     0 (0%)
Events:
  Type    Reason                   Age                From             Message
  ----    ------                   ----               ----             -------
  Normal  NodeHasSufficientMemory  17m (x3 over 17m)  kubelet          Node multi-node-k8s-worker status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    17m (x3 over 17m)  kubelet          Node multi-node-k8s-worker status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     17m (x3 over 17m)  kubelet          Node multi-node-k8s-worker status is now: NodeHasSufficientPID
  Normal  NodeAllocatableEnforced  17m                kubelet          Updated Node Allocatable limit across pods
  Normal  RegisteredNode           17m                node-controller  Node multi-node-k8s-worker event: Registered Node multi-node-k8s-worker in Controller
  Normal  NodeReady                17m                kubelet          Node multi-node-k8s-worker status is now: NodeReady
```

9. delete kind cluster

```
sudo kind delete cluster --name multi-node-k8s
```

example output

```
vagrant@vagrant:~/kind-cluster-multi-nodes$ sudo kind delete cluster --name multi-node-k8s
Deleting cluster "multi-node-k8s" ...
Deleted nodes: ["multi-node-k8s-worker2" "multi-node-k8s-control-plane" "multi-node-k8s-worker"]
```
