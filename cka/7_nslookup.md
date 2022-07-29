�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/
���� ���� : https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/

# ����
nginx-7 Deployment �� �����ϰ�, nslookup ��ƿ��Ƽ�� ����Ͽ� DNS records�� �����Ͻÿ�.
1. nginx-7 ��� �̸��� service�� expose �Ѵ�.
2. Ư�� DNS records�� ���� service �� pod �� access �� �� �ִ��� Ȯ���Ѵ�.
3. �� Deployment �� ��� Pod �� Container �� nginx image �� ����ؾ� �Ѵ�.
4. nslookup ��ƿ��Ƽ�� ����Ͽ� Service �� Pod�� DNS records �� ��ȸ�ϰ�, �� ������ ���� /opt/service.dns �� /opt/pod.dns�� �����Ѵ�.

# Ǯ��
1. Deployment ���� ��ɾ ���� �ɼ��� ���� �����Ѵ�.
* --image=nginx : nginx image �� ���� Pod �� �����Ѵ�.
```
kubectl create deploy nginx-7 --image=nginx
deployment.apps/nginx-7 created
```

2. expose ��ɾ ���� nginx-7 Deployment �� service �� �����Ų��.
* --port : Cluster ���ο��� ����� service ���� port
* --target-port : Service �� ���� �� Request �� Pod �Ǵ� Deployment �� �����ϱ� ���� port
```
kubectl expose deploy nginx-7 --name=nginx-7 --port=80 --target-port=80
service/nginx-7 exposed
```

3. DNS records �� ���� busybox pod �� �����Ѵ�.
```
$ echo 'apiVersion: v1
kind: Pod
metadata:
labels:
run: busybox
name: busybox
spec:
containers:
- image: busybox
  name: busybox
  command:
    - sleep
    - "3600"' > busybox.yaml

$ kubectl create -f busybox.yaml
pod/busybox created
```

4. nginx-7 Pod�� IP �� Ȯ���Ѵ�.
* �ش� ����Ʈ���� 171.18.0.6 �̴�.
* Pod �� ���Ƽ� �˻��� ����� �� �ڿ� " | grep nginx-7" �ɼ��� �߰��ؼ� Ư�� Pod �� �˻��Ѵ�.
```
$ kubectl get po -o wide
NAME                       READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
busybox                    1/1     Running   0          3m38s   172.18.0.7   minikube   <none>           <none>
nginx-7-744445cc6d-g8hrl   1/1     Running   0          20m     172.18.0.6   minikube   <none>           <none>

$ kubectl get po -o wide | grep nginx-7
nginx-7-744445cc6d-g8hrl   1/1     Running   0          22m     172.18.0.6   minikube   <none>           <none>
```

5. Service �� DNS records �� ��ȸ�ϰ� /opt/service.dns �� �����Ѵ�.
```
$ kubectl exec -it busybox -- nslookup nginx-7
Server:         10.96.0.10
Address:        10.96.0.10:53

Name:   nginx-7.default.svc.cluster.local
Address: 10.100.163.234

$ kubectl exec -it busybox -- nslookup nginx-7 > /opt/service.dns
```

6. Pod �� DNS records �� ��ȸ�ϰ� /opt/pod.dns �� �����Ѵ�.
* �������� A/AAAA records�� �����Ͽ� Pod IP �����ڸ� "-"�� �Ѵ�.
```
$ kubectl exec -it busybox -- nslookup 172-18-0-6.default.pod
Server:         10.96.0.10
Address:        10.96.0.10:53

$ kubectl exec -it busybox -- nslookup 172-18-0-6.default.pod > /opt/pod.dns
```