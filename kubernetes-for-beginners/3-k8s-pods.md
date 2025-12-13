# K8s Commands Quickstart - Pods

> I'm using Ubuntu OS when running these commands.

# A. Prerequisites

Check prerequisites from:

1. 1-local-k8s-with-kind.md
2. 2-docker-quickstart.md

Pull images from DockerHub:

```
sudo docker pull arm64v8/nginx:1.29-alpine-otel
sudo kind load docker-image arm64v8/nginx:1.29-alpine-otel --name multi-node-k8s

sudo docker pull arm64v8/redis:8.4.0-bookworm
sudo kind load docker-image arm64v8/redis:8.4.0-bookworm --name multi-node-k8s

sudo docker pull ridwanbejo/hello-world-go:v1.0.0
sudo kind load docker-image ridwanbejo/hello-world-go:v1.0.0 --name multi-node-k8s
```

install another required tool for Redis:

```
sudo apt install redis-tools
```

> Since I used ARM-based CPU, as far as I've tried, I can only load Docker ARM-based images. Other than that, it was failed.

# B. Imperative command examples

1. run pods with single container called my-nginx with image `arm64v8/nginx:1.29-alpine-otel`:
```
sudo kubectl run my-nginx --image=arm64v8/nginx:1.29-alpine-otel
```

example output:
```
vagrant@vagrant:~$ sudo kubectl run my-nginx --image=arm64v8/nginx:1.29-alpine-otel
pod/my-nginx created
```

2. listing all pods:

```
sudo kubectl get pods
```

example output:

```
vagrant@vagrant:~$ sudo kubectl get pods
NAME             READY   STATUS    RESTARTS      AGE
my-nginx         1/1     Running   0             9s
```

3. check pods `my-nginx` detail:

```
sudo kubectl describe pods my-nginx
```

example output:
```
vagrant@vagrant:~$ sudo kubectl describe pods my-nginx
Name:             my-nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             multi-node-k8s-worker/172.18.0.2
Start Time:       Sat, 13 Dec 2025 16:16:29 +0000
Labels:           run=my-nginx
Annotations:      <none>
Status:           Running
IP:               10.244.2.3
IPs:
  IP:  10.244.2.3
Containers:
  my-nginx:
    Container ID:   containerd://b61bac0b0e8622d26ac9e411a085fc96ad691bfdacf4beebd06678c55a30ea09
    Image:          arm64v8/nginx:1.29-alpine-otel
    Image ID:       sha256:f524a5a5ae4964e286f5f0324de8cc49b46dbaaddbd54a90291d6a575e736f72
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sat, 13 Dec 2025 16:16:31 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9rf8m (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-9rf8m:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  16m   default-scheduler  Successfully assigned default/my-nginx to multi-node-k8s-worker
  Normal  Pulled     16m   kubelet            Container image "arm64v8/nginx:1.29-alpine-otel" already present on machine
  Normal  Created    16m   kubelet            Created container: my-nginx
  Normal  Started    16m   kubelet            Started container my-nginx
```

4. check pods `my-nginx` logs:

```
sudo kubectl logs my-nginx
```

example output: 

```
vagrant@vagrant:~$ sudo kubectl logs my-nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/12/13 16:16:31 [notice] 1#1: using the "epoll" event method
2025/12/13 16:16:31 [notice] 1#1: nginx/1.29.4
2025/12/13 16:16:31 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2025/12/13 16:16:31 [notice] 1#1: OS: Linux 6.8.0-88-generic
2025/12/13 16:16:31 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/12/13 16:16:31 [notice] 1#1: start worker processes
2025/12/13 16:16:31 [notice] 1#1: start worker process 37
2025/12/13 16:16:31 [notice] 1#1: start worker process 38
2025/12/13 16:16:31 [notice] 1#1: start worker process 39
2025/12/13 16:16:31 [notice] 1#1: start worker process 40
2025/12/13 16:16:31 [notice] 1#1: start worker process 41
```

5. accessing console of pods `my-nginx`:

```
sudo kubectl exec -it my-nginx -- sh
```

example output:

```
vagrant@vagrant:~$ sudo kubectl exec -it my-nginx -- sh
/ # ps aux
PID   USER     TIME  COMMAND
    1 root      0:00 nginx: master process nginx -g daemon off;
   37 nginx     0:00 nginx: worker process
   38 nginx     0:00 nginx: worker process
   39 nginx     0:00 nginx: worker process
   40 nginx     0:00 nginx: worker process
   41 nginx     0:00 nginx: worker process
   48 root      0:00 sh
   54 root      0:00 ps aux
/ #
```

6. exposing pods to handle HTTP requests:

```
sudo kubectl port-forward pod/my-nginx 8080:80
```

example output:

```
vagrant@vagrant:~$ sudo kubectl port-forward pod/my-nginx 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

7. test pods `my-nginx` by hitting root URL:

```
curl localhost:8080/
```

example output:

```
vagrant@vagrant:~$ curl localhost:8080/
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

8. Repeat prior commands but for Redis. And see what is the output for every commands:

```
sudo kubectl run my-redis --image=arm64v8/redis:8.4.0-bookworm
sudo kubectl get pods
sudo kubectl describe pods my-redis
sudo kubectl logs my-redis
sudo kubectl exec -it my-redis -- bash
sudo kubectl port-forward pod/my-redis 6379:6379
```

9. Use redis-cli to access Redis on pods `my-redis` on different terminal window

```
redis-cli
```

example output:

```
vagrant@vagrant:~$ redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set testing fromvagrantbox
OK
127.0.0.1:6379> get testing
"fromvagrantbox"
127.0.0.1:6379>
```

check again from inside pods `my-redis`:

```
sudo kubectl exec -it my-redis -- bash
```

example output:

```
root@my-redis:/data# redis-cli
127.0.0.1:6379> get testing
"fromvagrantbox"
127.0.0.1:6379>
```

So, the key which set from vagrant as host is fetchable from redis-cli inside pods `my-redis` also.

10. Repeat prior commands but for hello-world-go. And see what is the output for every commands:

```
sudo kubectl run my-hello --image=ridwanbejo/hello-world-go:v1.0.0
sudo kubectl get pods
sudo kubectl describe pods my-hello
sudo kubectl logs my-hello
sudo kubectl exec -it my-hello -- sh
sudo kubectl port-forward pod/my-hello 8080:8080
```

on different terminal window, check pods `my-hello` with:

```
curl http://localhost:8080/
```

and see what happen next.

11. Delete all pods

```
sudo kubectl delete pods my-hello my-nginx my-redis
```

example output:

```
vagrant@vagrant:~$ sudo kubectl delete pods my-hello my-nginx my-redis
pod "my-hello" deleted from default namespace
pod "my-nginx" deleted from default namespace
pod "my-redis" deleted from default namespace
```

listing all pods again in default namespace:

```
vagrant@vagrant:~$ sudo kubectl get pods
No resources found in default namespace.
```

# C. YAML-based deployment

generate pods template:
```
sudo kubectl run my-nginx --image=arm64v8/nginx:1.29-alpine-otel --dry-run=client -o yaml > my-nginx.yaml
```

it will generate file like below:

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: my-nginx
  name: my-nginx
spec:
  containers:
  - image: arm64v8/nginx:1.29-alpine-otel
    name: my-nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

command to apply the pods YAML manifest:

```
sudo kubectl apply -f my-nginx.yaml
```

check again created pods:

```
vagrant@vagrant:~/Projects$ sudo kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
my-nginx   1/1     Running   0          17s

vagrant@vagrant:~/Projects$ sudo kubectl logs my-nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/12/13 17:08:58 [notice] 1#1: using the "epoll" event method
2025/12/13 17:08:58 [notice] 1#1: nginx/1.29.4
2025/12/13 17:08:58 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2025/12/13 17:08:58 [notice] 1#1: OS: Linux 6.8.0-88-generic
2025/12/13 17:08:58 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/12/13 17:08:58 [notice] 1#1: start worker processes
2025/12/13 17:08:58 [notice] 1#1: start worker process 37
2025/12/13 17:08:58 [notice] 1#1: start worker process 38
2025/12/13 17:08:58 [notice] 1#1: start worker process 39
2025/12/13 17:08:58 [notice] 1#1: start worker process 40
2025/12/13 17:08:58 [notice] 1#1: start worker process 41
```

Repeat these commands for pods `my-redis` and `my-hello`:

```
sudo kubectl run my-hello --image=ridwanbejo/hello-world-go:v1.0.0 --dry-run=client -o yaml > my-hello.yaml
sudo kubectl run my-redis --image=arm64v8/redis:8.4.0-bookworm --dry-run=client -o yaml > my-redis.yaml

sudo kubectl apply -f my-hello.yaml
sudo kubectl apply -f my-redis.yaml
```

Then, see what happened on your console.