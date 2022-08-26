# 설치 순서

## 소개
k3s 기반으로 제공한다. 

주피터 기반 소프트웨어 제공으로 ML개발을 집중할 수 있도록 제공한다. 

## 설치
설치순서는 docker 설치, k3s설치, docker image pull, jupyterhub설치 순으로 진행한다.

### 시스템 최신 업데이트 및 필수 패키지 설치
```sh
$ sudo apt-get update
$ sudo apt-get install -y curl ca-certificates
```
* NHN Toast Ubuntu 18.04.6 LTS with NVIDIA (2021.12.21) 이미지는 apt-get update 실행시 GPG 오류가 발생한다. fix 방법
```sh
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC
```

### docker 설치
```sh
$ curl https://get.docker.com | sh
$ service docker start
```

### nvidia-docker2 설치
```sh
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

$ sudo apt-get update

$ sudo apt-get install -y nvidia-docker2
```

### nvidia-docker2를 docker default-runtime으로 설정
**,"default-runtime": "nvidia"** 추가
```sh
$ sudo vi /etc/docker/daemon.json

{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
	,"default-runtime": "nvidia"
    }
}
```


### 1.k3s 설치 및 docker image pull
k3s의 설치는 아래 문서를 참조한다. 

CPU모델:  [CPU-INSTALL.md](CPU-INSTALL.md)
	
GPU모델:  [GPU-INSTALL.md](GPU-INSTALL.md)

K3s 서비스 확인
```sh
$ service k3s status
```

### 2.yaml 파일 다운로드
```sh
$ curl https://raw.githubusercontent.com/aiblabco/kamp-single/main/KAMP3.tar | tar xf -
```

### 3.Namespace 생성
```sh
$ kubectl create ns jupyterhub
```

### 4.jupyterhub 설치
```sh
$ kubectl apply -f kamp-helm.yaml
$ kubectl apply -f kamp-clusterrole.yaml
$ kubectl apply -f kampnote-lib.yaml
$ kubectl apply -f jhub-ingress.yaml
```

### 5.설치후 확인 
```sh
$ kubectl get pod -n jupyterhub
```
의 결과가 모든 pod에 대해 STATUS값이 Running 상태여야 합니다.

#### 6. 서버 접속
클라우드 VM의 ip를 브라우져에 입력하여 로그인화면 확인

```
http://Floating IP/kamp
https://Floating IP/kamp
```

#### 7. 기본 관리자 아이디/암호
관리자 아이디 : admin
관리자 비밀번호 : 1234


