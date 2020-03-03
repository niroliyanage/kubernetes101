# kubernetes101


Tools Needed

eksctl
kubectl
helm
aws-iam-authenticator  - https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html


Kube context switcher - https://github.com/ahmetb/kubectx

export AWS_PROFILE="nirothegreat"
export AWS_DEFAULT_REGION="ap-southeast-2"

## Run Sheet ##

eksctl create cluster -f eks.yml 


kubectl cluster-info


kubectl get clusterroles

kubectl describe configmap -n kube-system aws-auth

// kubectl edit -n kube-system configmap/aws-auth

kubectl apply -f awsauth.yaml


kubectl create namespace mywebsite
kubectl get namespaces # validate 
kubectl config set-context --current --namespace=my-website

### configure Helm

./install-helm

kubectl -n kube-system create serviceaccount tiller

kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

helm init --service-account tiller


kubectl get sa tiller

kubectl get  clusterrolebinding tiller -o yaml

kubectl get pods --namespace kube-system

helm repo list

#### Installing the First Chart

kubectl config set-context --current --namespace=kube-system

helm install stable/kubernetes-dashboard --name dashboard-demo

helm ls

cd dashboard

kubectl apply -f .

export POD_NAME=$(kubectl get pods -n kube-system -l "app=kubernetes-dashboard,release=dashboard-demo" -o jsonpath="{.items[0].metadata.name}")

kubectl -n kube-system describe secret eks-admin| awk '$1=="token:"{print $2}'

kubectl port-forward $POD_NAME 8443:8443


#### installing Wordpress

kubectl config set-context --current --namespace=my-website

helm install stable/wordpress

helm fetch  stable/wordpress --untar


### installing Prometheus/Grafana

kubectl config set-context --current --namespace=monitoring

helm install --name prometheus stable/prometheus

helm install --name grafana stable/grafana


## Installing an Ingress and an Ingress Controller


