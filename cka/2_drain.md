�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

## ����
Minikube Node�� ��� �Ұ����� ���·� �����, �� Node�� ���� ��� Pod�� �ٸ� Node�� �̰��Ͻÿ�.

# �������
* �� url�� �ǽ� ȯ���� Minikube ���� ȯ���̱� ������ �ٸ� Node���� Pods �̰��� �Ұ���������, ��ɾ �����µ� ���Ǹ� �д�.
* ���� Pods ���� Reschedule �Ǵ� ���� Ȯ���ϱ� ���� ������ Pod�� �����Ѵ�.

# Ǯ��
1. ��� Node�� ���� cordon ��ɾ �����Ѵ�. ���� kubectl get no ��ɾ ���� SchedulingDisabled ���·� ����Ǿ����� Ȯ���Ѵ�.
* cordon : �ش� Node�� �� �̻� Pod�� �ڵ����� Scheduling ���� �ʴ´�.
```
$ kubectl cordon minikube
node/minikube cordoned

$ kubectl get no
NAME       STATUS                     ROLES                  AGE   VERSION
minikube   Ready,SchedulingDisabled   control-plane,master   27m   v1.20.2
```

2. ��� Node�� ���� drain ��ɾ �����Ѵ�. ���� kubectl get po ��ɾ ���� �ش� Node�� Pods�� ��� Reschdule �Ǿ����� Ȯ���Ѵ�.
* drain : ��ó�� cordon ��ɾ ���� �Է����� �ʾƵ� drain ��ɾ �Է��ϸ� cordon�� ���� ����ǰ�, ���� Reschedule �ȴ�.
* --delete-emptydir-data : �������� --delete-local-data �ɼ��� deprecated �ǰ� �ش� �ɼ����� ��ü�Ǿ���.
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