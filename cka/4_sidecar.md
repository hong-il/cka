실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/
참고 문서 : https://kubernetes.io/docs/concepts/cluster-administration/logging/

# 문제(Built-in Logging Architecture)
Kube4 Pod에 busybox Image를 사용한 sidecar 컨테이너를 추가하시오.
이때, 신규 sidecar 컨테이너는 다음 명령어을 통해 로그를 생성한다.
* /bin/sh -c tail -n+1 -f /var/log/kube4.log

# 풀이
1. 참고 문서의 two-files-counter-pod.yaml을 참고하여 다음과 같이 입력한다.
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

2. 다음 명령어를 통해 Pod를 생성한다.
* create는 신규 생성 명렁어
* apply는 덮어쓰기 명렁어(신규 생성도 가능)
```
$ kubectl apply -f kube4.yaml
```

3다음 명령어를 통해 문서 수정 창으로 들어간다.
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

4. dd 명령어를 통해 필요 없는 라인을 삭제하고, insert 버튼을 눌러 다음과 같이 편집한다. 이후 esc + :wq 명령어로 저장하고 닫는다.
* 2yy 명령어로 현재 커서 기준 2줄을 복사할 수 있다.
* p 명령어로 현재 커서 기준 아랫줄에  yy로 복사한 행을 붙여넣을 수 있다.
* u 명령어로 본인의 액션을 undo 할 수 있다.
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

5. 기존에 동작 중이던 Pod를 삭제하고, 위에서 생성한 파일을 실행한다.
* --force 옵션을 사용하면 Pod가 삭제될 때까지 기다리지 않아도 된다.
* create는 신규 생성 명령어
* apply는 덮어쓰기 명령어(신규 생성도 가능)
```
$ kubectl delete po kube4 --force
$ kubectl apply -f kube4.yaml
pod/kube4 created
```

6생성한 Built-in Logging Architecture가 정상 작동하는 지 확인한다.
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