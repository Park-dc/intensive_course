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
