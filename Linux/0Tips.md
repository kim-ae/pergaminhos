Right ssh permissions:
#ssh #permissions #linux #chmod #cli
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```


Check service logs using journalctl
#journalctl #linux #cli #logs
```
journalctl --unit=kubelet.service
```

Check which process is locking a file
#fuser #lock #linux #cli #process
```
sudo fuser -v /var/cache/debconf/config.dat
```

Check listening ports
#netstat #linux #cli #ports #network
```
sudo netstat -ano | grep LISTEN
```
Force Resolve to use specific DNS
```
sudo resolvectl revert wlp0s20f3
sudo resolvectl dns wlp0s20f3 192.168.10.3   
```