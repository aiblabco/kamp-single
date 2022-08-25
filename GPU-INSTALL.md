# 설치

## NHN Toast 클라우드 기반 
설치 대상 인스턴스
- GPU Instance : Ubuntu Server 18.04.6 LTS with NVIDIA (2021.12.21)


```sh
$ sudo apt-get update
GPG Error ....
GPG Error 수정 방법
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC


$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
            
$ sudo apt-get update

$ sudo apt-get install -y curl ca-certificates


$ curl https://get.docker.com | sh

$ service docker start

$ sudo apt-get install -y nvidia-docker2

$ sudo vi /etc/docker/daemon.json
```

원본
```vi
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

default-runtime 추가
```vi
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    },
    "default-runtime": "nvidia"
}
```

```sh
$ sudo service docker restart
```

k3s install
```sh
$ curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.22.8+k3s1 K3S_KUBECONFIG_MODE="644" sh -s - --docker
```

nvidia-device-plugin install

nvidia-device-plugin.yaml
```yaml
apiVersion: helm.cattle.io/v1
kind: HelmChart
metadata:
  name: nvidia-device-plugin
  namespace: kube-system
spec:
  chart: nvidia-device-plugin
  repo: https://nvidia.github.io/k8s-device-plugin
```

```sh
$ kubectl apply -f nvidia-device-plugin.yaml
```



