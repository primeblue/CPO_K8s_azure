# AutoScaling
오토스케일링 구현을 위한 Metric 서버 설치와 HPA 구성후 테스트 실습

1. 실습 디렉토리 이동
```
cd /root/CPO_K8s_azure/lab4/yaml
```


# Metic-server 설치

```
kubectl create -f metric-server.yaml
```

# Deployment 편집

```
kubectl edit deployment metrics-server -n kube-system
```

```
/kubelet-use-node-status-port

- --kubelet-use-node-status-port
- --metric-resolution=15s
- --kubelet-insecure-tls
```
# 맨 아랫줄 추가(- --kubelet-insecure-tls)	
인증서가 공인 기관에 승인 받지 않은 안전하지 않기 때문에 보안적으로 취약하지만 무시하겠다는 의미


# 동작 확인

```
kubectl top node
```

# 실습에 필요한 Deployment와 Service 생성

```
kubectl create -f php-apache.yaml
```

# HPA 생성

```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

# 확인

```
kubectl get hpa
```

# 부하 생성(다른 터미널을 추가하여 진행)

```
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```

# 1분정도 뒤 확인

```
kubectl get hpa
kubectl get deployment php-apache
```


# 부하 중지(부하 생성 터미널에서 진행)

```
Ctrl + C
```

# 확인

```
kubectl get hpa
kubectl get deployment php-apache
```

참고 : [https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/](https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/)
