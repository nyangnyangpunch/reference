ubuntu 18.04, x86_64
==============
cpu 2개 이상 할당 필수

docker install
--------------
```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
apt-cache madison docker-ce

sudo docker run hello-world
```

dpkg frontend lock 해제
----------------------
```
sudo killall apt apt-get
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*
sudo dpkg --configure -a
sudo apt update
```

kubernetes install
------------------
```
sudo apt install apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo add-apt-repository "deb https://apt.kubernetes.io/ kubernetes-$(lsb_release -cs) main"
sudo add-apt-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt update
sudo apt install -y kubeadm kubelet kubectl kubernetes-cni

sudo swapoff -a 
sudo sysctl net.bridge.bridge-nf-call-iptables=1
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
노드 추가
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
포드 네트워크
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.10.0/Documentation/kube-flannel.yml
kubectl get nodes
```
not Ready일 시
```
sudo snap install kops
```

슬레이브 노드 추가
```
sudo kubeadm join 172.31.20.231:6443 --token 95bt0c.dqi8yv7xzhbqgwcp --discovery-token-ca-cert-hash sha256:b2c7c12685340f8782013b2fe0c1521c74f02994b9f15a068f13a38a39c114c0
kubectl get nodes
```
dashboard
```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
kubectl get pods --all-namespaces
kubectl -n kube-system get service kubernetes-dashboard
kubectl proxy --accept-hosts='^*
```
웹서버 프록시 설정
```
sudo apt install apache2
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_html
sudo a2enmod proxy_http
sudo a2enmod rewrite
sudo service apache2 restart
```
dashboard 접속
```
http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
https://도메인/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

kubectl -n kube-system describe $(kubectl -n kube-system \
    get secret -n kube-system -o name | grep namespace) | grep token
```

istio install
```
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.2.2 sh -
cd istio-1.2.2
export PATH=$PWD/bin:$PATH
for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```
deploy application할 때 istio 이용
```
kubectl label namespace <namespace> istio-injection=enabled
kubectl create -n <namespace> -f <your-app-spec>.yaml

istioctl kube-inject -f <your-app-spec>.yaml | kubectl apply -f -
```

elk 다음 주소 참조
https://github.com/pires/kubernetes-elk-cluster
