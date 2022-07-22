실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

## 문제
단일 컨테이너에 다음 3개의 Image를 포함한 kube1이라는 이름의 Pod를 만드시오.
* nginx
* redis
* memcached

## 풀이
1. Pod 생성 명령어에 다음 옵션들을 더해 실행한다.
* --image=nginx : nginx 이미지를 가진 Pod를 생성한다.
* --dry-run=client : 실제로 Node에 Pod 생성 분이 반영되지 않는다. 구버전의 --dry-run 옵션이 deprecated 되고 해당 옵션으로 대체되었다.
* -o yaml : 파일 포맷을 yaml로 지정한다.
* 생성 된 파일을 다음 디렉토리의 파일명으로 설정한다(디렉토리 미 입력 시 현재 위치).
```
$ kubectl run kube1 --image=nginx --dry-run=client -o yaml > kube1.yaml
```

2. 다음 명령어를 통해 문서 수정 창으로 들어간다.
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

3. dd 명령어를 통해 필요 없는 라인을 삭제하고, insert 버튼을 눌러 다음과 같이 편집한다. 이후 esc + :wq 명령어로 저장하고 닫는다.
* 2yy 명령어로 현재 커서 기준 2줄을 복사할 수 있다.
* p 명령어로 현재 커서 기준 아랫줄에  yy로 복사한 행을 붙여넣을 수 있다.
* u 명령어로 본인의 액션을 undo 할 수 있다.
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

4. 해당 파일을 실행한다.
* create는 신규 생성 명령어
* apply는 덮어쓰기 명령어(신규 생성도 가능)
```
$ kubectl apply -f kube1.yaml
```

5. 생성한 Pod가 정상적으로 작동 중인지, Image 3개가 정상적으로 생성되었는지 확인한다.
* STATUS가 Running 이면 정상
* READY가 3/3 이면 정상
```
$ kubectl get po
NAME                    READY   STATUS    RESTARTS   AGE
non-persistent-volume   1/1     Running   0          74s
```
