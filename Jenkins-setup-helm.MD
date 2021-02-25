### Jenkins server setup with Helm to deploy into Kubernetes cluster

## Download and Install helm 
```sh
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh -v v2.16.1
```

## Test with helm command
```sh
helm version
helm
```
## Enable 10250 on Master and Worker Nodes for helm communication
```sh
port: 10250
```
## Copy config file from Kubernetes master to Jenkins home directory
```sh
mkdir /var/lib/jenkins/.kube
copy config file (from below location) under .kube directory with jenkins ownership.
#kubeadm -> /etc/kubernetes/admin.conf
#CloudShell -> .kube/config

#if want to copy file (in place of copy/paste) to Jenkins server 
# on Jenkins server
vi /etc/ssh/sshd_config
  Enable PasswordAuthentication
service sshd restart
passwd ec2-user

From Console: scp .kube/config ec2-user@<jenkinsSIP>:/tmp

```

## Install tiller in Kubernetes master
```sh
kubectl -n kube-system create serviceaccount tiller
```
### Next, bind the tiller serviceaccount to the cluster-admin role:
```sh
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
```
## Execute below command in Jenkins server to install tiller on cluster
```sh
helm init --service-account tiller
# We may need to install gcloud if it fails. Needs Python 2.7, by default Amazon Linux comes with Pythin v2.6. 
```
## To verify that Tiller is running, list the pods in the kube-system namespace:
```sh
kubectl get pods --namespace kube-system
```







