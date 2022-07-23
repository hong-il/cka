�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# ����
������ ���� ������ ���� kube3��� �̸��� Deployment�� �����ϰ� ���纻�� /opt/kube3.yaml�� �����Ͻÿ�.
* 7���� Replicas�� ���� Nginx ����
* label�� app_runtime_stage=dev�� ����

# Ǯ��
1. Deployment ���� ��ɾ ���� �ɼǵ��� ���� �����Ѵ�.
* --imge=nginx : nginx �̹����� ���� Pod�� �����Ѵ�.
* --replicas : Node�� Pod ������ 7���� �����Ѵ�.
* --dry-run=client : ������ Node�� Pod ���� ���� �ݿ����� �ʴ´�. �������� --dry-run �ɼ��� deprecated �ǰ� �ش� �ɼ����� ��ü�Ǿ���.
* -o yaml : ���� ������ yaml�� �����Ѵ�.
* ���� �� ������ ���� ���丮�� ���ϸ����� �����Ѵ�(���丮 �� �Է� �� ���� ��ġ).
```
$ kubectl create deploy kube3 --image=nginx --replicas=7 --dry-run=client -o yaml > /opt/kube3.yaml
```

2. ���� ��ɾ ���� ���� ���� â���� ����.
```
$ vi /opt/kube3.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
creationTimestamp: null
labels:
app: kube3
name: kube3
spec:
replicas: 7
selector:
matchLabels:
app: kube3
strategy: {}
template:
metadata:
creationTimestamp: null
labels:
app: kube3
spec:
containers:
- image: nginx
name: nginx
resources: {}
status: {}
```

3. dd ��ɾ ���� �ʿ� ���� ������ �����ϰ�, insert ��ư�� ���� ������ ���� �����Ѵ�. ���� esc + :wq ��ɾ�� �����ϰ� �ݴ´�.
* 2yy ��ɾ�� ���� Ŀ�� ���� 2���� ������ �� �ִ�.
* p ��ɾ�� ���� Ŀ�� ���� �Ʒ��ٿ�  yy�� ������ ���� �ٿ����� �� �ִ�.
* u ��ɾ�� ������ �׼��� undo �� �� �ִ�.
```
apiVersion: apps/v1
kind: Deployment
metadata:
labels:
app_runtime_stage: dev
name: kube3
spec:
replicas: 7
selector:
matchLabels:
app_runtime_stage: dev
template:
metadata:
labels:
app_runtime_stage: dev
spec:
containers:
- image: nginx
name: nginx
```

4. �ش� ������ �����Ѵ�.
* create�� �ű� ���� ��ɾ�
* apply�� ����� ��ɾ�(�ű� ������ ����)
```
$ kubectl apply -f /opt/kube3.yaml
deployment.apps/kube3 created
```

5. ������ Deployment�� ���������� �۵� ������, Pod 7���� ���������� �����Ǿ����� Ȯ���Ѵ�.
```
$ kubectl get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
kube3   7/7     7            7           17s

$ kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
kube3-55c786d6bc-2fhhc   1/1     Running   0          19s
kube3-55c786d6bc-6lhrs   1/1     Running   0          19s
kube3-55c786d6bc-fsxjt   1/1     Running   0          19s
kube3-55c786d6bc-g8dnc   1/1     Running   0          19s
kube3-55c786d6bc-lz66z   1/1     Running   0          19s
kube3-55c786d6bc-s4h45   1/1     Running   0          19s
kube3-55c786d6bc-vdhz9   1/1     Running   0          19s
```