# jenkins -agent (with docker-host) 


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

  * https://www.jenkins.io/blog/2022/12/27/run-jenkins-agent-as-a-service/
