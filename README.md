# k3d_istio

step1. **asdf install plugin in .tool-versions**

![alt text](image-3.png)

```zsh
asdf plugin add k3d https://github.com/spencergilbert/asdf-k3d.git &&
asdf install
```
![alt text](image-1.png)

![alt text](image-2.png)
#
step2. **Create k3d cluster without traefik**

```zsh 
k3d cluster create DevOps --agents 2 --api-port 0.0.0.0:6443 -p '9080:80@loadbalancer' --k3s-arg "--disable=traefik@server:*"
```
![alt text](image-4.png)

* cluster name: DevOps
* cluster master nodes: 1
* cluster worker nodes: 2
* api-server works on port: 6443

note: if you are using a firewall you can allow this port through $ sudo ufw allow 6443
#
step2. **install istio**

```zsh 
helm install istio-base istio/base -n istio-system --create-namespace --set defaultRevision=default
helm install istiod istio/istiod -n istio-system --wait
helm install istio-ingress istio/gateway -n istio-ingress --create-namespace --wait
```
note: verification of established resources
```zsh
helm ls -n istio-system
helm status istio-base -n istio-system
helm status istiod -n istio-system
helm status istio-ingress -n istio-ingress
-----
helm get all istio-base -n istio-system
helm get all istiod -n istio-system
helm get all istio-ingress -n istio-ingress
```
note: Use the below command to label the default namespace with the Istio injection-enabled tag
```zsh 
kubectl label namespace default istio-injection=enabled
```
note: if u gonna use argocd to deploy those charts, u need to getch those repos local on ur side for more visibility
```zsh 
helm fetch istio/base --untar
helm fetch istio/istiod --untar
helm fetch istio/gateway --untar 
```
![alt text](image-5.png)