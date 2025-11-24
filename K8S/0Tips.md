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