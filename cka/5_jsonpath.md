�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# ����
Ư�� namespace�� ��� Pod�� ���� name �� namespace ������ json ǥ������ �̿��Ͽ� ��Ÿ���ÿ�.
* ���� �ذ��� ���� �ű� namespace�� Pod�� ���� �����Ѵ�.

# Ǯ��
1. web namespace�� �����Ѵ�.
```
$ kubectl create namespace web
```

2. web namespace�� nginx��� �̸��� Pod�� �����Ѵ�.
* -n : �ڿ� namespace ���� �Է��ϸ� �ش� namespace���� ��ɾ �����Ѵ�.
```
$ kubectl run nginx --image=nginx -n web
```

3. json ǥ������ �̿��Ͽ� ��� Pods�� �̸� �� namespace ������ ����Ѵ�.
* -n �ɼ��� ���� ������ default namespace�� Pod ����� ����ϰ� -n �ɼ� �ڿ� ����ϰ� ���� ��� namespace�� �Է��ϸ�, �ش� namespace�� Pod ����� ����Ѵ�.
```
$ kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}"

$ kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}" -n web
nginx web
```