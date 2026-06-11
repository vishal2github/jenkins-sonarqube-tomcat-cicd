# Jenkins setup (In separate EC2 instance)

### Step-1 (EC2 creation)

Name : Jenkins-server (Provide any name)  
AMI  : Ubuntu

Security Group → Add rule:  
 - Type	: Custom TCP
 - Port	: 8080
 - Source	: 0.0.0.0/0

---

### Step-2 (JDK, Jenkins installation)

`sudo apt update`

`sudo apt install openjdk-21-jdk -y`  
`java --version`

`curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2026.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null`

`echo "deb [trusted=yes] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null`

`sudo apt update`

`sudo apt install jenkins -y`  
`jenkins --version`

`sudo systemctl start jenkins`  
`sudo systemctl enable Jenkins`  
`sudo systemctl status Jenkins`

---

### Step-3 (Jenkins configuration)

Access Jenkins: `http://your-ec2-public-ip:8080`

Get Jenkin's initial password: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
	
Login using initial password, and install the suggested plugins option

Set up the jenkins user and save and continue/finish (Start using jenkins):  
 - Set "Username"
 - Set "Password"
 - Enter "Fullname"
 - Enter "Email"

---

### Step-4 (Check/Install/Configure required packages in EC2 instance)

Required packages: JENKINS, JDK21, GIT, MAVEN

Verify package installed locations:  
 - Jenkins : `/usr/bin/jenkins`			(If not found, install using `sudo apt install jenkins -y`)  
 - Git	: `/usr/bin/git`				(If not found, install using `sudo apt install git -y`)  
 - JDK	: `/usr/lib/jvm/java-21-openjdk-amd64`	(If not found, install using `sudo apt install openjdk-21-jdk -y`)  
 - Maven	: `/usr/share/maven`			(If not found, install using `sudo apt install maven -y`)


Maven manual installation via zip file (If not `sudo apt install maven`):  
 - Download file : `sudo wget https://dlcdn.apache.org/maven/mvnd/1.0.5/maven-mvnd-1.0.5-linux-aarch64.tar.gz`  
 - Extract file  : `sudo tar -xvf apache-maven-3.8.6-bin.tar.gz`

	... and ...

Maven manual installation as a root in /opt (If not `sudo apt install maven`):  
 - `sudo su`  
 - `cd /opt`  
 - `ls -l` (Check for any existing Maven)  
 - Download file : `wget https://dlcdn.apache.org/maven/maven-3/3.9.16/binaries/apache-maven-3.9.16-bin.tar.gz`  
 - Extract file  : `tar -xvzf apache-maven-3.9.16-bin.tar.gz`  

Verify 'bin/' content: `cd /opt/apache-maven-3.9.16/bin/`

---

### Step-5 (Jenkins Plugins configuration: Select and install manually, if not pre-installed)

Manage Jenkins > Plugins:  
 - JUnit Attachments
 - Git Server
 - Maven Integration
 - Deploy to container (war/ear)
 - SonarQube Scanner
 - SSH Agent

*Restart after plugins installations

---

### Step-6 (GitHub Authentication Elements)

GitHub:  
 - Settings > Developer settings
 - Personal Access token > Tokens (Classic)
 - Create 'PAT' Token (Set token name, expiration, Scopes [Check on repo header, leave all as unchecked unless asked for])

Goal:  
 - PAT 	        : Enter Personal-Access-Token here  
 - GitHub UN    : Enter GitHub username here  
 - Repo-Address : `https://github.com/<github_user>/<repo_name.git>` (Repository must be available already!)

Note: PAT is a password used in creation of Jenkins Credentials (Manage Jenkins > Credentials > Add Credential > Now create credential)

---

### Step-7 (Jenkins Tools Configuration)

Manage Jenkins > Tools

 - Maven Configuration:  
   - Default settings provider 	      : Use default maven settings  
   - Default global settings provider : Use default maven global settings

 - JDK installations:  
	- Name: jdk-17.0.18 (Use this command for suitable name: `java --version`)  
	- Uncheck "Install automatically" if pre-selected  
	- JAVA_HOME: `/usr/lib/jvm/java-17-openjdk-amd64`

 - GIT installation:  
	- Leave everything at default

 - Maven Installation:
	- Name: maven3.8.6 (Use this command for suitable name: `mvn -version`)
	- Uncheck "Install automatically" if pre-selected
	- MAVEN_HOME: `/usr/share/maven`

 - Apply and Save
 