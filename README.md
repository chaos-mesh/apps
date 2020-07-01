# apps
applications 

## Three centers demo

----------------------
|     |   busybox-0  | 
|IDC A|   busybox-1  |
|     |   busybox-2  |
----------------------

----------------------
|     |   busybox-3  | 
|IDC B|   busybox-4  |
|     |   busybox-5  |
----------------------

----------------------
|     |   busybox-6  | 
|IDC C|   busybox-7  |
|     |   busybox-8  |
----------------------
       10ms
IDC A <---->  IDC B
       20ms
IDC A <---->  IDC C
       30ms
IDC B <---->  IDC C
```
# Create apps run on three centers
kubectl apply -f https://raw.githubusercontent.com/chaos-mesh/apps/master/three-centers/busybox-statefulset.yaml

# Create network delays on three centers
kubectl apply -f https://raw.githubusercontent.com/chaos-mesh/apps/master/three-centers/network-delay.yaml

# Then you can ping different pod
kubectl exec busybox-0 -it -n busybox -- ping -c 2 busybox-3.busybox.busybox.svc
# PING busybox-3.busybox.busybox.svc (10.244.3.10): 56 data bytes
# 64 bytes from 10.244.3.10: seq=0 ttl=63 time=10.808 ms
# 64 bytes from 10.244.3.10: seq=1 ttl=63 time=10.419 ms

kubectl exec busybox-0 -it -n busybox -- ping -c 2 busybox-6.busybox.busybox.svc
# PING busybox-6.busybox.busybox.svc (10.244.3.11): 56 data bytes
# 64 bytes from 10.244.3.11: seq=0 ttl=63 time=20.118 ms
# 64 bytes from 10.244.3.11: seq=1 ttl=63 time=20.358 ms

kubectl exec busybox-3 -it -n busybox -- ping -c 2 busybox-6.busybox.busybox.svc
# PING busybox-6.busybox.busybox.svc (10.244.3.11): 56 data bytes
# 64 bytes from 10.244.3.11: seq=0 ttl=63 time=30.219 ms
# 64 bytes from 10.244.3.11: seq=1 ttl=63 time=30.367 ms
```