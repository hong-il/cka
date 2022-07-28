실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# 문제
특정 namespace의 모든 Pod에 대한 name 및 namespace 정보를 json 표현식을 이용하여 나타내시오.
* 문제 해결을 위해 신규 namespace에 Pod를 먼저 생성한다.

# 풀이
1. web namespace를 생성한다.
```
$ kubectl create namespace web
```

2. web namespace에 nginx라는 이름의 Pod를 생성한다.
* -n : 뒤에 namespace 명을 입력하면 해당 namespace에서 명령어를 실행한다.
```
$ kubectl run nginx --image=nginx -n web
```

3. json 표현식을 이용하여 모든 Pods의 이름 및 namespace 정보를 출력한다.
* -n 옵션을 주지 않으면 default namespace의 Pod 목록을 출력하고 -n 옵션 뒤에 출력하고 싶은 대상 namespace를 입력하면, 해당 namespace의 Pod 목록을 출력한다.
```
$ kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}"

$ kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}" -n web
nginx web
```