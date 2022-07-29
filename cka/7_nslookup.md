실습 가능 환경 : https://kubernetes.io/ko/docs/tutorials/hello-minikube/
참고 문서 : https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/

# 문제
nginx-7 Deployment 를 생성하고, nslookup 유틸리티를 사용하여 DNS records를 저장하시오.
1. nginx-7 라는 이름의 service를 expose 한다.
2. 특정 DNS records를 통해 service 및 pod 에 access 할 수 있는지 확인한다.
3. 이 Deployment 내 모든 Pod 의 Container 는 nginx image 를 사용해야 한다.
4. nslookup 유틸리티를 사용하여 Service 와 Pod의 DNS records 를 조회하고, 그 내용을 각각 /opt/service.dns 및 /opt/pod.dns에 저장한다.

# 풀이
1. Deployment 생성 명령어에 다음 옵션을 더해 실행한다.
* --image=nginx : nginx image 를 가진 Pod 를 생성한다.
```
kubectl create deploy nginx-7 --image=nginx
deployment.apps/nginx-7 created
```

2. expose 명령어를 통해 nginx-7 Deployment 를 service 로 노출시킨다.
* --port : Cluster 내부에서 사용할 service 전용 port
* --target-port : Service 로 전달 된 Request 를 Pod 또는 Deployment 로 전달하기 위한 port
```
kubectl expose deploy nginx-7 --name=nginx-7 --port=80 --target-port=80
service/nginx-7 exposed
```

3. DNS records 를 위한 busybox pod 를 생성한다.
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

4. nginx-7 Pod의 IP 를 확인한다.
* 해당 포스트에선 171.18.0.6 이다.
* Pod 가 많아서 검색이 어려울 땐 뒤에 " | grep nginx-7" 옵션을 추가해서 특정 Pod 만 검색한다.
```
$ kubectl get po -o wide
NAME                       READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
busybox                    1/1     Running   0          3m38s   172.18.0.7   minikube   <none>           <none>
nginx-7-744445cc6d-g8hrl   1/1     Running   0          20m     172.18.0.6   minikube   <none>           <none>

$ kubectl get po -o wide | grep nginx-7
nginx-7-744445cc6d-g8hrl   1/1     Running   0          22m     172.18.0.6   minikube   <none>           <none>
```

5. Service 의 DNS records 를 조회하고 /opt/service.dns 에 저장한다.
```
$ kubectl exec -it busybox -- nslookup nginx-7
Server:         10.96.0.10
Address:        10.96.0.10:53

Name:   nginx-7.default.svc.cluster.local
Address: 10.100.163.234

$ kubectl exec -it busybox -- nslookup nginx-7 > /opt/service.dns
```

6. Pod 의 DNS records 를 조회하고 /opt/pod.dns 에 저장한다.
* 참고문서의 A/AAAA records를 참고하여 Pod IP 구분자를 "-"로 한다.
```
$ kubectl exec -it busybox -- nslookup 172-18-0-6.default.pod
Server:         10.96.0.10
Address:        10.96.0.10:53

$ kubectl exec -it busybox -- nslookup 172-18-0-6.default.pod > /opt/pod.dns
```