# Tomcat-9 server setup (In separate EC2 instance)

### Step-1 (EC2 instance creation)

Name	      : Tomcat-server (Provide any name)  
AMI	          : Ubuntu  
Instance type : t2.micro

Security group rules:
 - SSH	      : 22
 - Custom TCP : 9090

---

### Step-2 (Install JDK17, Tomcat-9)

`sudo apt update -y`
	
`sudo yum install java-17-openjdk -y`  
`java --version`
	
`sudo apt install wget -y`  
`sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz`  
`sudo tar -xvf apache-tomcat-9.0.80.tar.gz`

---

### Step-3 (Tomcat user-ownership configuration and server startup)

`sudo chmod 777 /home/ubuntu/apache-tomcat-9.0.80`

`sudo chown -R ubuntu:ubuntu apache-tomcat-9.0.80`  
`./apache-tomcat-9.0.80/bin/startup.sh` (Start Tomcat server)

---

### Step-4 (Apache files configuration)

*Refer 'Tomcat_files_configuration.pdf' setup file from utilities folder
	
In `apache-tomcat-9.0.80/conf/server.xml`:
 - Modify port number: 9090
	
In `apache-tomcat-9.0.80/conf/tomcat-users.xml`:
```
<role rolename="admin-gui"/>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="enter_username" password="enter_password" roles="admin-gui,manager-gui,manager-script"/>
```

In `apache-tomcat-9.0.80/webapps/manager/META-INF/context.xml`:
 - allow=".*"

Start Tomcat server (Run `startup.sh`)
 - (Run `shutdown.sh` and then `startup.sh` for reset-restart)

Access 'Apache Tomcat' Homepage via `<instance-ip>:9090`  
Access 'Manager App' webpage to verify proper working of Tomcat server

---

### Step-5 (Jenkins Credentials for Tomcat)

In Jenkins server > Log into Jenkins webpage  
Manage Jenkins > Security > Credentials

Add Credentials:
 - Select 'SSH Username with private key'
    - ID 	      : ec2-tomcat-key (Give any optimum name)
    - Description : Leave blank
    - Username    : ubuntu
				
	- Select 'Private Key': Click 'Add' (Use 'cat' on AWS generated private key, copy the key content and paste it)
    - Click 'Create'

Verify 'ec2-tomcat-key' key in 'Credentials' list (To be eventually added in 'deploy' stage of pipeline)

Note: To use Tomcat with Jenkins, the 'ssh agent' and 'deploy to container' plugins must be installed in Jenkins.
