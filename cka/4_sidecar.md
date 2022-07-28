�ǽ� ���� ȯ�� : https://kubernetes.io/ko/docs/tutorials/hello-minikube/
���� ���� : https://kubernetes.io/docs/concepts/cluster-administration/logging/

# ����(Built-in Logging Architecture)
Kube4 Pod�� busybox Image�� ����� sidecar �����̳ʸ� �߰��Ͻÿ�.
�̶�, �ű� sidecar �����̳ʴ� ���� ��ɾ��� ���� �α׸� �����Ѵ�.
* /bin/sh -c tail -n+1 -f /var/log/kube4.log

# Ǯ��
1. ���� ������ two-files-counter-pod.yaml�� �����Ͽ� ������ ���� �Է��Ѵ�.
```
$ echo 'apiVersion: v1
kind: Pod
metadata:
name: kube4
spec:
containers:
- name: kube4
  image: busybox
  args:
    - /bin/sh
    - -c
    - >
      i=0;
      while true;
      do
      echo "$(date) INFO $i" >> /var/log/kube4.log;
      i=1;
      sleep 1;
      done
      volumeMounts:
    - name: logs
      mountPath: /var/log
      volumes:
- name: logs
  emptyDir: {}' > kube4.yaml
```

2. ���� ��ɾ ���� Pod�� �����Ѵ�.
* create�� �ű� ���� ����
* apply�� ����� ����(�ű� ������ ����)
```
$ kubectl apply -f kube4.yaml
```

3���� ��ɾ ���� ���� ���� â���� ����.
```
$ vi kube4.yaml
apiVersion: v1
kind: Pod
metadata:
name: kube4
spec:
containers:
- name: kube4
  image: busybox
  args:
    - /bin/sh
    - -c
    - >
      i=0;
      while true;
      do
      echo "$(date) INFO $i" >> /var/log/kube4.log;
      i=1;
      sleep 1;
      done
      volumeMounts:
    - name: logs
      mountPath: /var/log
      volumes:
- name: logs
  emptyDir: {}
```

4. dd ��ɾ ���� �ʿ� ���� ������ �����ϰ�, insert ��ư�� ���� ������ ���� �����Ѵ�. ���� esc + :wq ��ɾ�� �����ϰ� �ݴ´�.
* 2yy ��ɾ�� ���� Ŀ�� ���� 2���� ������ �� �ִ�.
* p ��ɾ�� ���� Ŀ�� ���� �Ʒ��ٿ�  yy�� ������ ���� �ٿ����� �� �ִ�.
* u ��ɾ�� ������ �׼��� undo �� �� �ִ�.
```
apiVersion: v1
kind: Pod
metadata:
name: kube4
spec:
containers:
- name: kube4
  image: busybox
  args:
    - /bin/sh
    - -c
    - >
      i=0;
      while true;
      do
      echo "$(date) INFO $i" >> /var/log/kube4.log;
      i=1;
      sleep 1;
      done
      volumeMounts:
    - name: logs
      mountPath: /var/log
- name: sidecar
  image: busybox
  args: [/bin/sh, -c, 'tail -n+1 -f /var/log/kube4.log']
  volumeMounts:
    - name: logs
      mountPath: /var/log
      volumes:
- name: logs
  emptyDir: {}
```

5. ������ ���� ���̴� Pod�� �����ϰ�, ������ ������ ������ �����Ѵ�.
* --force �ɼ��� ����ϸ� Pod�� ������ ������ ��ٸ��� �ʾƵ� �ȴ�.
* create�� �ű� ���� ��ɾ�
* apply�� ����� ��ɾ�(�ű� ������ ����)
```
$ kubectl delete po kube4 --force
$ kubectl apply -f kube4.yaml
pod/kube4 created
```

6������ Built-in Logging Architecture�� ���� �۵��ϴ� �� Ȯ���Ѵ�.
```
$ kubectl logs kube4 sidecar
Fri Jul 15 07:46:16 UTC 2022 INFO 0
Fri Jul 15 07:46:17 UTC 2022 INFO 1
Fri Jul 15 07:46:18 UTC 2022 INFO 1
Fri Jul 15 07:46:19 UTC 2022 INFO 1
Fri Jul 15 07:46:20 UTC 2022 INFO 1
Fri Jul 15 07:46:21 UTC 2022 INFO 1
Fri Jul 15 07:46:22 UTC 2022 INFO 1
```