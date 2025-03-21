# Taints and Tolerations:

. Basically it is used to restrict the unwanted pods in the specific node

.let us consider a secenario that the cluster having 3 nodes. if one of the node is working with AI releated models and having  a GPU's with taint gpu=true

.If when the pod is created the node-1 will checks the toleration gpu=true in the pod if ti has it will create in node-1 else it will create in another nodes.

.Taint will work on nodes level and tolerations are work in pods.

. Tolaration have key-value pais and also have something like EFFECT

# Taints and Tolerations architecture

![Alt Text](https://miro.medium.com/v2/resize:fit:708/1*yE-fFUw9mE_TZDvS8mIDGQ.png)

# OutPut 

```bash
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get nodes
NAME                          STATUS   ROLES           AGE   VERSION
cka-cluster-1-control-plane   Ready    control-plane   45h   v1.32.2
cka-cluster-1-worker          Ready    <none>          45h   v1.32.2
cka-cluster-1-worker2         Ready    <none>          45h   v1.32.2
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl taint nodes cka-cluster-1-worker gpu=true:NoSchedule
node/cka-cluster-1-worker tainted
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl taint nodes cka-cluster-1-worker2 gpu=true:NoSchedule
node/cka-cluster-1-worker2 tainted
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl run nginx-po --image=nginx
pod/nginx-po created
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
nginx-po   0/1     Pending   0          5s
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
nginx-po   0/1     Pending   0          13s
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl describe pod nginx-po
Name:             nginx-po
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           run=nginx-po
Annotations:      <none>
Status:           Pending
IP:
IPs:              <none>
Containers:
  nginx-po:
    Image:        nginx
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-gl4wj (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-gl4wj:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
QoS Class:                   BestEffort
Node-Selectors:              <none>
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  91s   default-scheduler  0/3 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 2 node(s) had untolerated taint {gpu: true}. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling.
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl delete pod nginx-po
pod "nginx-po" deleted
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl apply -f nginx.yaml
pod/nginx created
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          5s
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          46s
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl run redis --image=redis 
pod/redis created
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          85s
redis   0/1     Pending   0          7s
ubuntu@yugandhar:/mnt/c/Users/YUGANDHAR/OneDrive/Documents/GitHub/kubernetes-learning/day-14$ kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          2m43s   10.244.2.4   cka-cluster-1-worker   <none>           <none>
redis   0/1     Pending   0          85s     <none>       <none>                 <none>           <none>
```





