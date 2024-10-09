Deploy Jenkins on AWS ec2 instance

Create a EC2 Instance and login to it.

```
ssh -i key.pem ubuntu@Ipaddress
```
install jdk for jenkins

```
sudo apt update
sudo apt install openjdk-11-jdk
java -version
```
jenkins installation on ubuntu

```
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
```
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```
check jenkins status
```
systemctl status jenkins
```
by default jenkins run on port 8080 so edit inbound rules on security groups: type- custumtcp, port- 8080
```
http:/ipaddress:8080 
```
once jenkins accesable copy address and get password
```
sudo cat pasteaddress
```