
# Deployed an End-to-End Web-App on Kubernetes by using Docker, EC2, Minikube, Route 53.

## Setup and management of a Kubernetes-hosted docker container for a Django application

* Open aws console and launch an  EC2 t2.medium instance, we chose t2.medium beacuse we need 2 cpu to use Kubernetes

* Connect to the instance

      sudo apt-get update

## Docker Installation

* Install docker on ubuntu, we need docker to run Minikube

*  Set up the repository

        sudo apt-get install \
        ca-certificates \
        curl \
        gnupg


*       sudo mkdir -m 0755 -p /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

* Use the following command to set up the repository:

       echo \
       "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
       "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


* Install Docker Engine

        sudo apt-get update

        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


## Minikube Installation

        curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
      sudo install minikube-linux-amd64 /usr/local/bin/minikube


      minikube start --driver=docker

      sudo usermod -aG docker $USER && newgrp docker



* To Interact with your cluster

      sudo snap install kubectl
      sudo snap install kubectl --classic
      kubectl get po -A
      minikube status
      kubectl get node


* To Start Minikube

       minikube start


* Clone to repo to get all the files in the system

      git clone https://github.com/Pankajzire/kubernetes_django.git


      cd kubernetes_Django

* To build image

      docker build -t pankajzire/kubernetes_django_todo:latest .

* To run the image in container

      docker run -d -p 8000:8000 pankajzire/kubernetes_django_todo


* To check it is working or not

      curl -L http://x.y.z.w:8000

* To create kubernetes pod create a folder

      mkdir k8s

      cd k8s

* To push image to docker hub we need to login to docker hub

      docker login 

* Push the image


      docker images

      docker push kubernetes_django_todo:latest

* Create pod
     
      
      vim pod.yaml

     
*     apiVersion: v1

      kind: Pod

      metadata:

         name: To-Do

      spec:

         containers:

             - name: Todo-app

               image: pankajzire/kubernetes_django_todo:latest

               ports:

                 - containerPort: 8000

* Kill the running docker container for port availability  

      sudo docker kill "image-name"

* To build pod

      kubectl app -f pod.yaml

* To see runnig pods
      
      kubectl get pods 

* To get the IP
      
      kubectl get pods -O wide

* login to minikube and curl the IP

      minikube ssh

      curl -L http://IP:8000

      

## To do deployment, Replication, Auto-healing, Auto-scaling

* Create a deploymet file

      vim deployment.yaml

*     apiVersion: apps/v1

      kind: Deployment

      metadata:

         name: todo-deployment

         labels:

             app: todo-app

      spec:

         replicas: 3

         selector:

             matchLabels:

                 app: todo-app

         template:

             metadata:

                 labels:

                     app: todo-app

             spec:

                 containers:

                 - name: todo-app

                   image: pankajzire/kubernetes_django_todo:latest

                   ports:

                   - containerPort: 8000
     

* To deploy the deployment file

      kubectl apply -f deployment.yaml

* To see running deployments

      kubectl get deployments

* To delete pods

      kubectl delete pods "pod-name"


## To manage network and services with Host IP allocation through proxy on AWS EC2 and Route 53

* We still can't hit the URL as this is running inside of minikube

* So we need to create a service nodeport file
  
      vim service.yaml

*     apiVersion: v1
      kind: Service
      metadata:
         name: todo-service
      spec:
         type: NodePort
         selector:
             app: todo-app
        ports:
            # By default and for convenience, the `targetPort` is set to the same value as the `port` field.
             - port: 80
               targetPort: 8000
               # Optional field
               # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
               nodePort: 30007

* To apply service file

       kubectl apply -f service.yaml

* To see the running service

       kubectl get svc 

* To get external IP

      minikube service todo-service --url 

      or 

      minikube ip

* Run a curl command on the ip you get

      curl -L http://x.y.z.w:30007

* To give this IP a Hostname

      sudo vim /etc/hosts

* Scroll down till bottom and enter the IP that you got earlier

      x.y.z.w todo-app.com

* now curl with new Hostname

      curl -L http://todo-app.com:30007
    

    






