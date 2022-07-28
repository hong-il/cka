�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# ����
web deployment�� 7���� Pod�� Ȯ���Ͻÿ�.
* Replicas �ɼ��� ���� web deployment�� ���� ����

# Ǯ��
1. Deployment ���� ��ɾ ���� �ɼ��� ���� �����Ѵ�.
* --imge=nginx : nginx image �� ���� Pod�� �����Ѵ�.
```
$ kubectl create deploy web --image=nginx
deployment.apps/web created
```

2. Scale ��ɾ ����Ͽ� web Deployment�� 7���� Pods�� Ȯ���Ų��.
```
$ kubectl scale deploy web --replicas=7
deployment.apps/web scaled
```

3. ������ Deployment�� ���������� �۵� ������, Pod 7���� ���������� �����Ǿ����� Ȯ���Ѵ�.
```
$ kubectl get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
web     7/7     7            7           4m45s

$ kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
web-6799fc88d8-2w7l6     1/1     Running   0          84s
web-6799fc88d8-57dqg     1/1     Running   0          84s
web-6799fc88d8-6wz8k     1/1     Running   0          84s
web-6799fc88d8-ggc4l     1/1     Running   0          84s
web-6799fc88d8-gmf2h     1/1     Running   0          4m50s
web-6799fc88d8-pztk6     1/1     Running   0          84s
web-6799fc88d8-zh4lw     1/1     Running   0          84s
```