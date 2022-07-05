# Task 2 - Loadbalancing on ingess

### Ingress 의 구현체인 Contorller 설치와 로드밸런싱 실습
#


1. 웹브라우저로 아래 링크에 접속하여 ingress controller 설치 전 확인
```
https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
```

링크처럼 1개의 yaml 파일안에 여러 리소스를 구현할 수 있습니다.

2. nginx ingress controller 설치
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/baremetal/deploy.yaml
kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
```  
위 명령어처럼 링크에 있는 yaml파일도 kubectl을 통해 제어할 수 있습니다.

3. 홈페이지에 관한 Service, Pod yaml 확인
```
cat homepage.yaml
```

4. 위 yaml 로 리소스 생성
```
kubectl create -f homepage.yaml
```

5. 고객센터에 관한 Service, Pod yaml 확인
```
cat customer.yaml
```

6. 위 yaml 로 리소스 생성
```
kubectl create -f customer.yaml
```

7. 연간 스케쥴 페이지에 관한 Service, Pod yaml 확인
```
cat schedule.yaml
```

8. 위 yaml 로 리소스 생성
```
kubectl create -f schedule.yaml
```

9. 로드밸런싱 용 ingress yaml 확인
```
cat lb-ingress.yaml
```

10. 위 yaml 로 리소스 생성
```
kubectl create -f lb-ingress.yaml
```

11. 위에서 생성한 리소스 확인
```
kubectl get po
kubectl get service
kubectl get ingress
```

12. ingress controller 가 만든 Nodeport 유형의 서비스의 Nodeport를 확인
```
kubectl get svc -n ingress-nginx
```

13. 노드의 ip와 13과정에서 확인한 Nodeport를 확인하여 접근시도
```
curl <node ip>:<13에서 확인한 portnumber>
curl <node ip>:<13에서 확인한 portnumber>/customer
curl <node ip>:<13에서 확인한 portnumber>/schedule
```

15. clear
```
kubectl delete pod,svc,ingress --all
```
