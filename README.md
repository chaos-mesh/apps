# apps

## Three centers demo

Data center server topography

|  DC-A | DC-B | DC-C |
| ---- | ---- | ---- |
| busybox-0 | busybox-3 | busybox-6 |
| busybox-1 | busybox-4 | busybox-7 |
| busybox-2 | busybox-5 | busybox-8 |

The latency between different IDC
| DCs | Latency |
| ---- | ---- |
| A <--> B | 10ms |
| A <--> C | 20ms |
| B <--> C | 30ms |


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
