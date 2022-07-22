�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

## ����
���� �����̳ʿ� ���� 3���� Image�� ������ kube1�̶�� �̸��� Pod�� ����ÿ�.
* nginx
* redis
* memcached

## Ǯ��
1. Pod ���� ��ɾ ���� �ɼǵ��� ���� �����Ѵ�.
* --image=nginx : nginx �̹����� ���� Pod�� �����Ѵ�.
* --dry-run=client : ������ Node�� Pod ���� ���� �ݿ����� �ʴ´�. �������� --dry-run �ɼ��� deprecated �ǰ� �ش� �ɼ����� ��ü�Ǿ���.
* -o yaml : ���� ������ yaml�� �����Ѵ�.
* ���� �� ������ ���� ���丮�� ���ϸ����� �����Ѵ�(���丮 �� �Է� �� ���� ��ġ).
```
$ kubectl run kube1 --image=nginx --dry-run=client -o yaml > kube1.yaml
```

2. ���� ��ɾ ���� ���� ���� â���� ����.
```
$ vi kube1.yaml
apiVersion: v1
kind: Pod
metadata:
creationTimestamp: null
labels:
run: kube1
name: kube1
spec:
containers:
- image: nginx
  name: kube1
  resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  status: {}
```

3. dd ��ɾ ���� �ʿ� ���� ������ �����ϰ�, insert ��ư�� ���� ������ ���� �����Ѵ�. ���� esc + :wq ��ɾ�� �����ϰ� �ݴ´�.
* 2yy ��ɾ�� ���� Ŀ�� ���� 2���� ������ �� �ִ�.
* p ��ɾ�� ���� Ŀ�� ���� �Ʒ��ٿ�  yy�� ������ ���� �ٿ����� �� �ִ�.
* u ��ɾ�� ������ �׼��� undo �� �� �ִ�.
```
apiVersion: v1
kind: Pod
metadata:
labels:
run: kube1
name: kube1
spec:
containers:
- image: nginx
  name: nginx
- image: redis
  name: redis
- image: memcached
  name: memcached
```

4. �ش� ������ �����Ѵ�.
* create�� �ű� ���� ��ɾ�
* apply�� ����� ��ɾ�(�ű� ������ ����)
```
$ kubectl apply -f kube1.yaml
```

5. ������ Pod�� ���������� �۵� ������, Image 3���� ���������� �����Ǿ����� Ȯ���Ѵ�.
* STATUS�� Running �̸� ����
* READY�� 3/3 �̸� ����
```
$ kubectl get po
NAME                    READY   STATUS    RESTARTS   AGE
non-persistent-volume   1/1     Running   0          74s
```
