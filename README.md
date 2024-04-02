# k3d_istio_nginx

step1. **asdf install plugin in .tool-versions**

![alt text](image-3.png)

```zsh
asdf plugin add k3d https://github.com/spencergilbert/asdf-k3d.git &&
asdf install
```
![alt text](image-1.png)

![alt text](image-2.png)


step2. **Create k3d cluster without traefik**

```zsh 
k3d cluster create DevOps --agents 2 --api-port 0.0.0.0:6443 -p '9080:80@loadbalancer' --k3s-arg "--disable=traefik@server:*"
```