실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# 문제
다음과 같은 조건을 가진 kube3라는 이름의 Deployment를 생성하고 복사본을 /opt/kube3.yaml에 저장하시오.
* 7개의 Replicas를 가진 Nginx 설정
* label은 app_runtime_stage=dev로 설정

# 풀이
1. Deployment 생성 명령어에 다음 옵션들을 더해 실행한다.
* --imge=nginx : nginx 이미지를 가진 Pod를 생성한다.
* --replicas : Node에 Pod 복제본 7개를 생성한다.
* --dry-run=client : 실제로 Node에 Pod 생성 분이 반영되지 않는다. 구버전의 --dry-run 옵션이 deprecated 되고 해당 옵션으로 대체되었다.
* -o yaml : 파일 포맷을 yaml로 지정한다.
* 생성 된 파일을 다음 디렉토리의 파일명으로 설정한다(디렉토리 미 입력 시 현재 위치).
```
$ kubectl create deploy kube3 --image=nginx --replicas=7 --dry-run=client -o yaml > /opt/kube3.yaml
```

2. 다음 명령어를 통해 문서 수정 창으로 들어간다.
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

3. dd 명령어를 통해 필요 없는 라인을 삭제하고, insert 버튼을 눌러 다음과 같이 편집한다. 이후 esc + :wq 명령어로 저장하고 닫는다.
* 2yy 명령어로 현재 커서 기준 2줄을 복사할 수 있다.
* p 명령어로 현재 커서 기준 아랫줄에  yy로 복사한 행을 붙여넣을 수 있다.
* u 명령어로 본인의 액션을 undo 할 수 있다.
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

4. 해당 파일을 실행한다.
* create는 신규 생성 명령어
* apply는 덮어쓰기 명령어(신규 생성도 가능)
```
$ kubectl apply -f /opt/kube3.yaml
deployment.apps/kube3 created
```

5. 생성한 Deployment가 정상적으로 작동 중인지, Pod 7개가 정상적으로 생성되었는지 확인한다.
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