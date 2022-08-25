# 설치 순서

## 소개
k3s 기반으로 제공한다.
주피터 기반 소프트웨어 제공으로 ML개발을 집중할 수 있도록 제공한다.

## 설치 조건
설치 :
조건 : 

```
OS : ubuntu 18.04
메모리 : 8GB 이상
VCPU : 4 core이상
STORAGE : 100GB 이상
설치 및 이용을 위한 Floating IP 필요
방화벽 포트 오픈 : http(80), https(443) (ssh – 설치를 위한 접근 포트)
클라우드 VM ssh 접근
```
## 설치
설치순서는 k3s설치, docker image pull, jupyterhub설치 순으로 진행한다.

### 1.k3s 설치 및 docker image pull
k3s의 설치는 아래 문서를 참조한다. 

CPU모델:  CPU-INSTALL.md
	
GPU모델:  GPU-INSTALL.md

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


