#Installing Cloud SDK

apt-get install apt-transport-https ca-certificates gnupg

echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

sudo apt-get update && sudo apt-get install google-cloud-sdk

#view zone list
gcloud compute zones list

#Set a default compute zone and region from the list with the UP status.
gcloud config set compute/region us-west1

#INSTALL KUBECTL
apt-get install kubectl
kubectl version --short --client

gcloud init 
gcloud auth list
gcloud compute instances list

#view nodes cluster kubernet
kubectl get nodes

#Start cluster
#Enable the Google container service.
gcloud services enable container.googleapis.com


Deploy NGINX Web Server

kubectl create deployment nginx --image=nginx --replicas=3

#check
kubectl get pods

#Create loadbalance
kubectl expose deployment nginx --name=nginx --type=LoadBalancer --port=80 --target-port=80

#check nginx
kubectl get svc nginx

https://cloud.google.com/sdk/docs/install#deb

#INSTALL HELM

curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm

https://helm.sh/docs/intro/install/


#INSTALL TERRAFORM on Debian/Ubuntu
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

Tips: install on Kali linux
vi /etc/apt/source.list
#add
deb [arch=amd64] https://apt.releases.hashicorp.com buster main
apt-get update && sudo apt-get install terraform

Example:
mkdir learn-terraform-docker-container
vi main.tf
With main.tf
#####
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.13.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}

#####
terraform init
sudo terraform apply 
http://localhost:8000/


#other
export GOOGLE_APPLICATION_CREDENTIALS=1bae78c1f6015c1d958ab78e4bbdb7272ffaa5eb

gcloud auth print-access-token | helm registry login -u 107504063644215188606 \
--password-stdin https://asia-southeast1-docker.pkg.dev

107504063644215188606

asia-southeast1-docker.pkg.dev/summer-rope-326016/quickstart-helm-repo

https://cloud.google.com/artifact-registry/docs/helm/quickstart#console