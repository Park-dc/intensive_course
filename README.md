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

## Linux에 kubectl 설치
```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
