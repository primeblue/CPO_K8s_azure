
1. kubeadm 초기화
```
sudo -i 
kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=<instace 사설 ip>
```
출력결과(kubeadm join 이하 명령어)는 worker node를 다른 노드를 사용하여 연동할 때 사용하는 명령어 입니다.
3~5분 소요됩니다.
![image](https://user-images.githubusercontent.com/92773629/137877948-678049de-4e17-4e11-be31-00daee62ef62.png)




3. Master 노드 설정
사용자에 대해 kubectl이 작동하도록 설정
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf
```

4. Calico 네트워크 플러그인 설치
```
kubectl create -f https://projectcalico.docs.tigera.io/manifests/tigera-operator.yaml
kubectl create -f https://projectcalico.docs.tigera.io/manifests/custom-resources.yaml

```
pod 정상 배포 확인
watch kubectl get pods -n calico-system


5. kubectl 자동완성 적용
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
source /etc/bash_completion
alias k=kubectl
complete -F __start_kubectl k
```
6. master node에도 pod 를 생성 할 수 있도록 설정
```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

7. node 확인
```
kubectl get nodes
```

8. git clone
```
git clone https://github.com/primeblue/CPO_K8s_azure
```
