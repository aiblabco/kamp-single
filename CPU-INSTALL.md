# 설치

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

### docker 설치
```sh
$ sudo apt-get update
$ sudo apt-get install -y curl
```

### k3s install
```sh
$ curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.22.8+k3s1 K3S_KUBECONFIG_MODE="644" sh -s –
```

#### docker image pull
```sh
$ sudo ctr image pull docker.io/aiblabco/jupyterhub-single:1.0.2
$ sudo ctr image pull docker.io/aiblabco/jupyterlab:1.0.14
$ sudo ctr image pull docker.io/aiblabco/jupyterlab-gpu:1.0.14
$ sudo ctr image pull docker.io/aiblabco/kampnote-single:1.0.1
```


