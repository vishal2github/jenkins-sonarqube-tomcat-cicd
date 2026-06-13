# SonarQube setup (In separate EC2 instance)

### Step-1 (EC2 instance creation)

Name	      : SQ-server (Provide any name)  
AMI	          : Amazon Linux 2023 kernel-6.18AMI  
Instance type : t2.medium

Security group rules:
 - SSH	      : 22  
 - Custom TCP : 9000

---

### Step-2 (Install wget, zip/unzip, JDK17, SonarQube)

`sudo yum update -y`

`sudo yum install wget -y`  
`sudo yum install zip -y`

`sudo yum install java-17-openjdk` or `sudo yum install -y java-17-amazon-corretto-devel`  
`java --version`

`sudo su`  
`cd /opt`
	
`wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.3.79811.zip`  
`unzip sonarqube-9.9.3.79811.zip`

`ls` (Check for extracted folder 'sonarqube-9.9.3.79811')  
Optional: Using 'mv', change name of default extract folder to 'sonarqube' for simplicity

---

### Step-3 (SonarQube user creation and ownership configuration)

SonarQube cannot run as root. Create a user (e.g., sonar):
 - `useradd sonar` (Or can use `useradd -d /opt/sonarqube -g sonar sonar`)
 - `cat /etc/passwd` (Confirm 'sonar' user)

Change ownership:
 - `chown -R sonar:sonar /opt/sonarqube-9.9.3.79811`
 - `chmod -R 755 /opt/sonarqube-9.9.3.79811`

Modify 'visudo':
 - Go for line: ## Allow root to run any commands anywhere (Line No. 99)
 - Add line below root line: `sonar    ALL=(ALL)    NOPASSWD:ALL`

---

### Step-4 (Start and access SonarQube)
	
`su - sonar`  
`cd /opt/`  
`ls` (Verify for extracted folder)

`./sonarqube-9.9.0.65466/bin/linux-x86-64/sonar.sh start`  
 - At this point, remember to run `sonar.sh` file with respective user only (In this case is 'sonar', and not 'root' or any other user)

Access webpage via `http://<server_public_ip>:9000`

Default credentials window:
 - username: Default username is `admin`
 - password: Default password is `admin`
 - Set new password

SonarQube Homepage appears

---

### Step-5 (SonarQube: Security token generation)

SonarQube homepage (via url) > Administrator > Security  
Under Tokens > Generate Tokens
 - Name 	   : sonar-token
 - Type 	   : Global Analysis Token
 - Expire in : Set expiration date
 - Click 'Generate'

---

### Step-6 (Jenkins System Configuration: SonarQube)

In Jenkins server > Log into Jenkins webpage  
Manage Jenkins > System

SonarQube servers:
 - Check 'Environment variables'

 - Click AddSonarQube:
   - Name 	    : Sonar-Server-9.9
   - Server URL : `http://<sonar-instance-ip>:9000` (This configuration changes everytime server is started/rebooted)
			
 - Server authentication token:
   - Click '+ Add' button > Add Credentials
				
 - Select 'Secret text' > Next
   - Scope  : Global (Jenkins, nodes, items, all child items, etc)
   - Secret : # Paste the token created in Step-5: SonarQube webpage > Administrator > Security

Apply and Save

---

### Step-7 (Jenkins Tools Configuration: SonarQube)

Manage Jenkins > Tools
	
SonarQube Scanner installations:
 - Click 'Add SonarQube Scanner installations' button:
   - Name: Sonar-Scanner-8.1
   - Check Install automatically: Select latest version possible (Eg: SonarQube Scanner 8.1.0.6389)
 - Apply and Save


Note: To use SonarQube with Jenkins, the 'SonarQube Scanner' plugin must be installed in Jenkins.
