

# django-todo
A simple todo app built with django

![todo App](https://raw.githubusercontent.com/shreys7/django-todo/develop/staticfiles/todoApp.png)
### Setup 1 :To run manually use these steps, To run on Docker directly jump to setup 2




* Launch EC2 Instance(ubuntu) connect to it 
* To get this repository, run the following command inside your git enabled terminal
```bash
 git clone https://github.com/Pankajzire/django-todo.git
```
* You will need django to be installed in you computer to run this app. 

* run the following command

      cd django-todo   
*
      sudo apt install python3-pip -y
*      
      pip install django
     
```bash
 python3 manage.py makemigrations
```

* This will create all the migrations file (database migrations) required to run this App.

* Now, to apply this migrations run the following command
```bash
 python3 manage.py migrate
```

* One last step and then our todo App will be live. We need to create an admin user to run this App. On the terminal, type the following command and provide username, password and email for the admin user
```bash
 python3 manage.py createsuperuser
```

* That was pretty simple, right? Now let's make the App live. We just need to start the server now and then we can start using our simple todo App. Start the server by following command

```bash
 python3 manage.py runserver 0.0.0.0:8001
```

* Once the server is hosted, head over to http://0.0.0.0:8000/todos for the App.
* To keep it running 

       nohup python3 manage.py runserver 0.0.0.0:8000

#### Setup 2 : Run using Docker. 
* Launch EC2 Instance(ubuntu) connect to it 
 
* To get this repository, run the following command inside your git enabled terminal

```bash
 git clone https://github.com/Pankajzire/django-todo.git
```
* To install Docker

      sudo apt  install docker.io -y

* To list Process running on port 8001 (if followed step 1)
    
      lsof -i:8001
    
* To kill the previous running (if followed step 1)

      kill "enter PID of the process"
    
* To Create Docker file

      vi Dockerfile

* Paste following in vi tab

      FROM python:3
      RUN pip install django==4.1.7
      COPY . .
      EXPOSE 8000
      CMD ["python","manage.py","runserver","0.0.0.0:8000"]

* To build Docker Image

      sudo docker build . -t todo-app

* To list running Docker Images

      sudo docker ps
      
* To run Docker Image using port(-p) to map ports

      sudo docker run -p 8001:8001 "enter container ID"
      
* To keep it Running use -d (docker deamon) wit it

      sudo docker run -d -p  8000:8000 "enter container ID"



#### Setup 3: Deploy End-to-End Web-App by a CICD Pipeline using GitHub, Docker, Jenkin, AWS.

* Launch EC2 Instance(ubuntu) connect to it 

1. IInstall Java:

* Update your system
 
 *     sudo apt update

* Install java

*     sudo apt install openjdk-11-jre -y

* Validate Installation

*     java -version

* It should look something like this

*       openjdk version "11.0.12" 2021-07-20 OpenJDK RuntimeEnvironment (build 11.0.12+7-post-Debian-2) OpenJDK 64-Bit Server VM (build 11.0.12+7-post-Debian-2, mixed mode, sharing)

Step - 2 Install Jenkins

* Just copy these commands and paste them onto your terminal.


*     curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \   /usr/share/keyrings/jenkins-keyring.asc > /dev/null 
*     echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \   https://pkg.jenkins.io/debian binary/ | sudo tee \   /etc/apt/sources.list.d/jenkins.list > /dev/null
*     sudo apt-get update 
*     sudo apt-get install jenkins -y


Step -3 Start jenkins

*     sudo systemctl enable jenkins
*     sudo systemctl start jenkins
*     sudo systemctl status jenkins

Step - 4 Open port 8080 from AWS Console

* take public IP of your Instance Open it in new tab with :8080

*  to get the password use this command
           
       sudo cat /var/lib/jenkins/secrets/initialAdminPassword
      
* Install suggested plugins

* Install git in your system     

      sudo apt-get install git
      
* Install Docker 

      sudo apt install docker.io -y

* Install Docker compose
      
      sudo apt install docker-compose

* docker compose file
      
      version : "3.3"
      services : 
          web : 
              build : .
              ports : 
                - 8000:8000


* to docker compose file is valid or not 

      sudo docker-compose config


## Jenkins Github integration 

1. Open Github on your browser
2. Click on Profile > Settings >  Developer settings >  Personal access tokens > Generate new tokens
* Note- jenkins-cicd
3. select :
* repo
* workflow
4. Generate token
5. copy that token and save it later you cannot access it

## Run Jenkins project

1. select new item 
2. name- to do list > Freestyle project
3. Source Code Management- script from SCM 
4. SCM- Git
5. Repository URL- paste the Github url of Repository where you store the files
6. Credentials > Add > Jenkins 
7. Kind - secret text
8. secret text- personal access token that you created
9. branch- main
10. Build Steps > add steps > Execute shell
11. write following commands 

    
*     sudo docker-compose down
*     sudo docker-compose up -d --force-recreate --no-deps --build web

10. create 
11. build
12. Copy Public IP of your instance open it in new tab with :8000

