## Debug node with kubectl debug
#access-node #debug
```
 kubectl debug node/<nodename> -it --image=ubuntu
```
/host <- will have host filesystem
### You do not have access to host namespace
#access-node #debug #linux #chroot #AKS
```
chroot /host /bin/systemctl list-units --type=service
```

## Force delete
#pod #kubectl #cli #delete #k8s
```
kubectl delete pod <pod-name> --grace-period=0 --force
```

## Helm Rollback
#cli #k8s #helm #rollback 
```
helm rollback <Helm name> <generation> -n <namespace>
```

## Useful pods
### Check TLS connection
#cli #k8s #pod #openssl #tls
```
kubectl run checkssl --image=alpine/openssl --restart=Never -it --command -- /bin/sh 
```

CPU Wait time Metric: container_pressure_cpu_waiting_seconds_total