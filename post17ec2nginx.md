## Installing NGINX as during AWS EC2 cloud-init

In this post, I'm documenting a series of sample commands that can install NGINX on an AWS Linux EC2 instance, and at the same time customize the default web page. The below commands can be copied and pasted into User Data field when creating the EC2 instance. As a pre-requisite, the EC2 needs to have Internet access to download NGINX.


```
sudo amazon-linux-extras enable nginx1
sudo yum clean metadata
sudo yum -y install nginx
sudo systemctl start nginx.service
sudo chkconfig nginx on
sudo rm /usr/share/nginx/html/index.html
sudo echo 'Hello World from'" $(hostname -f)" > /home/ec2-user/index.html
sudo cp /home/ec2-user/index.html /usr/share/nginx/html/index.html

```

Another example of how to do this for Apache is described [here](https://cloudkatha.com/how-to-use-ec2-user-data-script-to-install-apache-web-server)
