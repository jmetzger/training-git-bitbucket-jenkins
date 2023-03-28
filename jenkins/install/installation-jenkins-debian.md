# Script to install jenkins on debian as package with service 

```
#!/bin/bash 

groupadd sshadmin
USERS="11trainingdo"
echo $USERS
for USER in $USERS
do
  echo "Adding user $USER"
  useradd -s /bin/bash --create-home $USER
  usermod -aG sshadmin $USER
  echo "$USER:somesupersecretpassword" | chpasswd
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

echo "AllowGroups sshadmin" >> /etc/ssh/sshd_config 
systemctl reload sshd 

echo "Installing jenkins"
echo "Configuring gpg key"
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | gpg --dearmor > /etc/apt/trusted.gpg.d/jenkins.gpg
echo "Enabling jenkins repo"
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install -y lsof openjdk-11-jdk-headless jenkins

# it is already automatially started 
# systemctl start jenkins
# systemctl status jenkins
```
