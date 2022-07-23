실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

## 문제
Minikube Node를 사용 불가능한 상태로 만들고, 그 Node에 속한 모든 Pod를 다른 Node로 이관하시오.

# 참고사항
* 위 url의 실습 환경은 Minikube 단일 환경이기 때문에 다른 Node로의 Pods 이관은 불가능하지만, 명령어를 익히는데 의의를 둔다.
* 실제 Pods 들이 Reschedule 되는 것을 확인하기 위해 임의의 Pod를 생성한다.

# 풀이
1. 대상 Node에 대해 cordon 명령어를 실행한다. 이후 kubectl get no 명령어를 통해 SchedulingDisabled 상태로 변경되었는지 확인한다.
* cordon : 해당 Node에 더 이상 Pod가 자동으로 Scheduling 되지 않는다.
```
$ kubectl cordon minikube
node/minikube cordoned

$ kubectl get no
NAME       STATUS                     ROLES                  AGE   VERSION
minikube   Ready,SchedulingDisabled   control-plane,master   27m   v1.20.2
```

2. 대상 Node에 대해 drain 명령어를 실행한다. 이후 kubectl get po 명령어를 통해 해당 Node의 Pods가 모두 Reschdule 되었는지 확인한다.
* drain : 위처럼 cordon 명령어를 따로 입력하지 않아도 drain 명령어를 입력하면 cordon이 먼저 실행되고, 이후 Reschedule 된다.
* --delete-emptydir-data : 구버전의 --delete-local-data 옵션이 deprecated 되고 해당 옵션으로 대체되었다.
```
$ kubectl drain minikube --delete-emptydir-data --ignore-daemonsets --force
node/minikube already cordoned
WARNING: deleting Pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet: default/kube1, kube-system/storage-provisioner; ignoring DaemonSet-managed Pods: kube-system/kube-proxy-5xjqh
evicting pod kubernetes-dashboard/kubernetes-dashboard-968bcb79-ckdpc
evicting pod kube-system/metrics-server-56c4f8c9d6-qbrms
evicting pod kube-system/storage-provisioner
evicting pod kubernetes-dashboard/dashboard-metrics-scraper-f6647bd8c-54rmk
evicting pod default/kube1
evicting pod kube-system/coredns-74ff55c5b-7vcdm
pod/kubernetes-dashboard-968bcb79-ckdpc evicted
pod/metrics-server-56c4f8c9d6-qbrms evicted
pod/storage-provisioner evicted
pod/dashboard-metrics-scraper-f6647bd8c-54rmk evicted
pod/coredns-74ff55c5b-7vcdm evicted
pod/kube1 evicted
node/minikube evicted

$ kubectl get po
No resources found in default namespace.
```