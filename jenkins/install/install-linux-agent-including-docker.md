# jenkins -agent (with docker-host) 

## Step 1: Create new machine (virtual machine) 

```
#!/bin/bash

#### This need to be run as root 

groupadd sshadmin
USERS="11trainingdo"
echo $USERS
for USER in $USERS
do
  echo "Adding user $USER"
  useradd -s /bin/bash --create-home $USER
  usermod -aG sshadmin $USER
  echo "$USER:11dortmund22" | chpasswd
done

# We can sudo with 11trainingdo
usermod -aG sudo 11trainingdo 

# 20.04 and 22.04 this will be in the subfolder
if [ -f /etc/ssh/sshd_config.d/50-cloud-init.conf ]
then
  sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config.d/50-cloud-init.conf
fi

## both is needed 
sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config

usermod -aG sshadmin root

# TBD - Delete AllowUsers Entries with sed 
# otherwice we cannot login by group 

echo "AllowGroups sshadmin" >> /etc/ssh/sshd_config 
systemctl reload sshd 

### Now the docker / jenkins part 

apt-get update
apt install -y --no-install-recommends openjdk-17-jdk-headless

# adding jenkins user 
useradd -m -s /bin/bash jenkins 

# needed for installation of agent.jar
apt-get install -y ca-certificates curl gnupg lsb-release

mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# groupadd docker 
usermod -aG docker jenkins
usermod -aG sudo jenkins

## Alreaday install the service, although we need to do some manual steps 
mkdir -p /usr/local/jenkins-service
chown jenkins /usr/local/jenkins-service

cat << EOF > /etc/systemd/system/jenkins-agent.service

[Unit]
Description=Jenkins Agent

[Service]
User=jenkins
WorkingDirectory=/home/jenkins
ExecStart=/bin/bash /usr/local/jenkins-service/start-agent.sh
Restart=always

[Install]
WantedBy=multi-user.target

EOF


# not sure, if daemon-reload is really needed on ubuntu 22.04 LTS 
systemctl daemon-reload
systemctl enable jenkins-agent 

```

## Step 2: neuen Agent in jenkins anlegen 

  * http://164.90.173.76:8080/computer/


## Step 3: java-Client in install-agent.sh /usr/local/jenkins-agent installieren und rechte setzen 

```
# Achtung ip und anderen schlüssel
# also root
cd /usr/local/jenkins-service
vi start-agent.sh 
```


```
# Example you get from you jenkins 
# under new node 

```

```
#!/bin/bash
cd /usr/local/jenkins-service
# Just in case we would have upgraded the controller, we need to make sure that the agent is using the latest version of the agent.jar
curl -sO http://my_ip:8080/jnlpJars/agent.jar
java -jar agent.jar -jnlpUrl http://my_ip:8080/computer/My%20New%20Ubuntu%2022%2E04%20Node%20with%20Java%20and%20Docker%20installed/jenkins-agent.jnlp -secret my_secret -workDir "/home/jenkins"
exit 0

```

```
chmod u+x start-agent.sh 
chown -R jenkins:jenkins /usr/local/jenkins-service
chown -R jenkins:jenkins /home/jenkins 
systemctl start jenkins-agent 
systemctl status jenkins-agent
```


  * https://www.jenkins.io/blog/2022/12/27/run-jenkins-agent-as-a-service/



