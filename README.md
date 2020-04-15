# intensive_course

## Windows 10에 Linux 설치
### PowserShell을 관리자 권한으로 실행하여 WSL 활성화
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

### PC 재시작

### Windows 10에 Linux 설치
시작메뉴>Microsoft Stroe에서 ubuntu 검색 후, 18.04 version설치

### Ubuntu의 Archive Repository Server를 국내로 설정

```
sudo vi /etc/apt/sources.list
:%s/archive.ubuntu.com/ftp.daumkakao.com/g
:wq!
```

## Linux에 JDK 설치

### 설치 명령
```
sudo apt-get update
sudo apt install default-jdk
```

### JAVA_HOME 설정
 .bashrc에 export JAVA_HOME='usr/lib/jvm/java-11-openjdk-amd64' 추가
 
### 실행 path 추가
.bashrc에 export PATH=$PATH:$JAVA_HOME/bin:. 추가

### 수정사항 반영
```
source ~/.bashrc
```

### 설치 확인
```
echo $JAVA_HOME
java -version
```

## Docker Client 설치
실습시 이부분은 꼭 안해도 될듯
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce
sudo usermod -aG docker
```

## kubectl 설치
```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

## Azure-Cli 설치
```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az login
```

## Azure AKS connection
### Local에서 Azure AKS 연결
```
az aks get-credentials --resource-group MY_RESOURCE_GROUP --name MY_CLUSTER_NAME
```
### AKS 연결 확인
```
kubectl config current-context
kubectl get all
```

## Login Azure Container Registry
Docker 데몬이 설치되어 있지 않으면 이 부분 진행 안됨. 그러나 실기 테스트에서는 굳이 로컬에서 Docker 데몬을 구동할 필요 없음으로 패스해도 된다.
### Azure CLI
```
az acr login --name ACR_NAME
```
### 생성확인
```
cat ~/.docker/config.json
```

## Integrate AKS with ACR
```
az aks update -n CLUSTER_NAME -g RESOURCE_GROUP_NAME --attach-acr ACR_NAME
```

## Spring Boot 프로젝트 만들 때 추가할 라이브러리들
- JPA
- H2
- Rest Repository

## helm 및 kafka 설치
### helm 설치
```
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
kubectl --namespace kube-system create sa tiller      # helm 의 설치관리자를 위한 시스템 사용자 생성
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
helm init --service-account tiller
helm repo update
```

### kafka 설치
```
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
helm repo update
helm install --name my-kafka --namespace kafka incubator/kafka
```

### kafka 토픽 생성
```
kubectl -n kafka exec my-kafka-0 -- /usr/bin/kafka-topics --zookeeper my-kafka-zookeeper:2181 --topic eventTopic --create --partitions 1 --replication-factor 1
```

### kafka 이벤트 발행
```
kubectl -n kafka exec -ti my-kafka-0 -- /usr/bin/kafka-console-producer --broker-list my-kafka:9092 --topic eventTopic

```

### kafka 이벤트 수신
```
kubectl -n kafka exec -ti my-kafka-0 -- /usr/bin/kafka-console-consumer --bootstrap-server my-kafka:9092 --topic eventTopic --from-beginning
```
