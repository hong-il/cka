실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/

# 문제
web deployment를 7개의 Pod로 확장하시오.
* Replicas 옵션이 없는 web deployment를 먼저 생성

# 풀이
1. Deployment 생성 명령어에 다음 옵션을 더해 실행한다.
* --imge=nginx : nginx image 를 가진 Pod를 생성한다.
```
$ kubectl create deploy web --image=nginx
deployment.apps/web created
```

2. Scale 명령어를 사용하여 web Deployment를 7개의 Pods로 확장시킨다.
```
$ kubectl scale deploy web --replicas=7
deployment.apps/web scaled
```

3. 생성한 Deployment가 정상적으로 작동 중인지, Pod 7개가 정상적으로 생성되었는지 확인한다.
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