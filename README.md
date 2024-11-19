# Kubernetes_SBS
## Deployment

Minikube installation

```bash
sudo apt install git -y
sudo -i
git clone https://github.com/sandervanugt/kubestep.git
useradd -m -s /bin/bash user1
usermod -aG sudo user1
su - user1
./minikube-docker-setup.sh
minikube start
kubectl get all
```

Nginx Pod

```bash
kubectl --help
source <(kubectl completion bash)
kubectl run myapp --image=nginx
kubectl run -h | less 
kubectl get pods
```
Running stand-alone pods
```bash
kubectl run web --image=nginx
kubectl get pods
kubectl get all
kubectl describe pod web
kubectl logs web
kubectl delete pod web
kubectl get all
```
Running Deployments
```bash
kubectl create deploy myweb --image=nginx:1.17 --replicas=3
kubectl get all
kubectl delete pod myweb[Tab]
kubectl get all
kubectl set image deploy myweb nginx=nginx:1.21 # update the version of NGINX
kubectl get all
```
Working with YAML Files
```bash
kubectl create deploy myapp --image=nginx:1.21 --replicas=3 --dry-run=client -o yaml>myapp.yaml
kubectl apply -f myapp.yaml
nano myapp.yaml # change to replicas=2
kubectl apply -f myapp.yaml
kubectl get deploy myapp -o yaml | less
kubectl get all
kubectl delete -f myapp.yaml
```
Kubernetes Troubleshooting
```bash
kubectl create deploy mydb --image=mariadb
kubectl get pods
kubectl describe pod mydb[Tab]
kubectl logs mydb[Tab]
kubectl set env deploy mydb MARIADB_ROOT_PASSWORD=password
kubectl get all --selector app=mydb
kubectl set env deploy mydb MARIADB_ROOT_PASSWORD=password
kubectl get all
kubectl get all --show-labels # filter by labels
kubectl get all --selector app=mydb
``` 
