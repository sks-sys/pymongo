# pymongo
**Setup mongoDB**

Task1: MongoDB Installation
1. Install the latest stable version of MongoDB on your local system 
2. Your MongoDB installation should run on background 
3. Your MongoDB application should restart in case of any process failure. (Preferably monitored by some process monitoring tool eg. Systemd)

Install mongodb
sudo apt install mongodb


start the mongodb service
sudo systemctl start mongodb


Enable mongodb service
sudo systemctl enable mongodb


Check mongodb status
sudo systemctl status mongodb


To run mongodb in background
mongod



Configure /lib/systemd/system/mongodb.services in case of process failure
Add this line at the end of [Service] save and exit.

Restart=on-failure 


And run this command 
systemctl daemon-reload

Run mangoDB
mongo

To list all databases
Show dbs

Task 2: Previledges and Access
On the same installation create two different databases, with 3 different permissions sets with a strong password 
a. Database1 with read-only permissions 

use Database1


db.createCollection("test1");


db.createUser(
{ 
 user: "user1",
 pwd:  "Stong_Password@1",
 roles:
 [
 { role:"read",db:"Database1"}
 ] } );



b. Database2 with read/write permissions on database and tables 

use Database2


db.createCollection("test2");


db.createUser(
{ 
 user: "user2",
 pwd:  "Stong_Password@2",
 roles:
 [
 { role:"readWrite",db:"Database2"}
 ] } );



c. A different user with admin user permission on all database
db.createUser(
{   user: "user3",
    pwd: "Stong_Password@3",

    roles:[{role: "userAdminAnyDatabase" , db:"admin"}]})




2. Insert simple data using MongoDB CLI on two different databases you created
Data insertion in Database1 
db.Database1.insert({
    'name': 'jon snow',
    'address': 'winter fell',
    'tag': 'greatest swordman'
 })



Data insertion in Database1 
db.Database2.insert({
    'name': 'daenerys targaryen',
    'address': 'house of targaryen',
    'tag': 'mother of dragon'
 })





**Setup Docker**

**Setup Jenkins**
Docker Task

Application 
1. Write a Dockerfile that can run a Python application. 
a. Python application can be open source 
b. Use multi-stage build to write the Dockerfile

main.py
import pymongo

if __name__ == '__main__':
   print("hello from mongodb")
   client = pymongo.MongoClient("mongodb://localhost:27017/")
   print(client)
   db = client["Database1"]




Dockerfile
#Dockerfile, Image, Container

FROM python:3.8
ADD main.py .

RUN pip install pymongo

CMD ["python", "./main.py"]


To build docker image
sudo docker build -t pymongo .





Database 
1. The python application should connect to MongoDB 
a. Host/Run MongoDB on your local machine 
b. Follow “DevOps-Task_MongoDB-Setup-and-Configuration.pdf” guidelines for a complete set of instructions.



To start mongodb
mongod



To host mongodb
localhost:27017


Note: Please follow the MongoDB Task document. 

Deployment 
1. Install/Configure Jenkins on your local system 
2. Jenkins should be able to pull the latest code and build the image on your local 
3. Deploy and run the container application on your system

To install jenkins with JDK 8

sudo apt-get update


sudo apt-get install openjdk-8-jdk


wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -


sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'


sudo apt-get update


sudo apt-get install jenkins


sudo apt install git

 
Surf url  localhost:8080 to configure jenkins

Configure jenkins with secret key from /var/lib/jenkins/secrets/initialAdminPassword.txt
Copy the password and paste in the url, install all suggested features and you will get jenkins dashboard.





Jenkinsfile
pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sks-sys/pymongo.git']]])
            }
        }
        stage('build') {
            steps {
                sh 'docker build . -t pymongo'
            }
        }
        stage('publish') {
            steps {
               echo 'tested successfully!!'
            }
        }
    }
}




