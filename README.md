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
Exposing Applications with Services
```bash
kubectl get deploy,pods
kubectl expose deploy myweb --port=80
kubectl get svc,deploy,pods --show-labels
curl <service-cluster-ip> #no access
kubectl delete svc myweb
kubectl expose deploy myweb --port=80 --type=NodePort
curl <node-IP-address>:<nodeport>
```
Configuring Ingress
```bash
minikube addons enable ingress
minikube addons list
kubectl get all -n kube-system
sudo sh -c "echo $(minikube ip) myweb.example.com >> /etc/hosts"
kubectl create ing myweb --rule="myweb.example.com/=myweb:80"
kubectl describe ing myweb
curl myweb.example.com
```
Deploying Pods with Volumes
```bash
git clone https://github.com/sandervanvugt/kubestep.git
cd kubestep
kubectl apply -f podvol.yaml
kubectl describe pod podvol
kubectl exec podvol -c busybox1 -- touch /busy1/testfile
kubectl exec podvol -c busybox2 -- ls -l /busy2/
```
Using PVCs and PVs
```bash
git clone https://github.com/sandervanvugt/kubestep.git
cd ~/kubestep
kubectl create namespace myvol
kubectl apply -f pv-pvc-pod.yaml
kubectl exec -n myvol local-pv-pod -- touch /usr/share/nginx/html/testfile
kubectl describe pv local-pv-volume
minikube ssh
ls /mnt/data/
```
Using StorageClass in Minikube
```bash
kubectl get storageclass
kubectl describe storageclass
kubectl get pods -n kube-system
kubectl apply -f pvc.yaml
kubectl get pvc,pv
```
Using Variables from ConfigMaps
```bash
kubectl create cm mydbvars1 --from-literal=MARIADB_ROOT_PASSWORD=password
kubectl create deploy mynewdb1 --image=mariadb
kubectl get all --selector=mynewdb1
kubectl set env --from=configmap/mydbvars deploy mynewdb
kubectl get all --selector=mynewdb
kubectl describe pod mynewdb[Tab]
```
Using Configuration from ConfigMaps
```bash
echo hello configmap>myindex.html
kubectl create cm myidex --from-file=myindex.html
kubectl create deploy mynewweb --image=nginx
```
Using Passwords from Secrets
```bash
kubectl create secret generic mysecretdbpw --from-literal=MARIADB_ROOT_PASWORD=password
kubectl get secret mysecretdbpw -o yaml
kubectl set env --from=secret/mysecretdbpw deploy/mydb
kubectl get pod mydb[Tab] -o yaml | less
```
Using Passwords from Secrets
```bash
kubectl create secret generic mysecretdbpw --from-literal=MARIADB_ROOT_PASWORD=password
kubectl get secret mysecretdbpw -o yaml
kubectl set env --from=secret/mysecretdbpw deploy/mydb
kubectl get pod mydb[Tab] -o yaml | less
``` 
