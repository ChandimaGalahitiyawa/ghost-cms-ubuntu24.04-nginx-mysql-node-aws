# Ghost CMS Installation on AWS EC2 (Ubuntu 24.04 + nginx + node (NVM) + MySQL + certbot)

Discover the ultimate step-by-step guide for installing and setting up Ghost CMS on an AWS EC2 instance with Ubuntu 24.04. This detailed tutorial covers everything from configuring nginx and nvm via Node.js to setting up MySQL, Ghost, and securing your site with certbot for SSL. Whether you're using DigitalOcean, Contabo, Vultr, AWS Lightsail, or any other cloud service with VPS, this guide is designed to help you seamlessly deploy Ghost CMS for a robust and secure blogging platform. Enhance your website's performance and security with our expert installation guide tailored for modern cloud services.

## Prerequisites

- An AWS account
- Basic knowledge of SSH and command-line operations
- A domain name (optional, but recommended for SSL)

## Step 1: Launch an EC2 Instance

1. Go to the AWS Management Console.
2. Launch an EC2 instance with Ubuntu 24.04 as the AMI.
3. Choose an instance type (e.g., t2.micro).
4. Configure security group to allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) traffic.

## Step 2: Connect

Use AWS Console


Use SSH to connect to your instance. (Terminal or Putty)
```
ssh -i your-key.pem ubuntu@your-ec2-instance-ip
```



## Step 3: Add user

Create a new user and follow prompts
```
adduser chandima
```

Add user to superuser group to unlock admin privileges
```
usermod -aG sudo chandima
```

change user
```
su - chandima
```

## Step 4: Install Softwares
```
cd ~ && sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get install nginx -y && sudo ufw allow 'Nginx Full' && sudo apt-get install mysql-server -y && sudo apt-get install ufw -y && sudo apt install certbot python3-certbot-nginx -y && cd ~ && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash && source ~/.bashrc && nvm install v18.13.0 && npm install ghost-cli@latest -g
```

## Step 5: mysql Setup
Enter mysql
```
sudo mysql
```

mysql root password
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH 'mysql_native_password' BY 'Strong_password';FLUSH PRIVILEGES;exit;
```

## Ste 6: Create Directory

change permissions linux
```
chmod 755 /home/chandima
```

Create directory: Change sitename to whatever you like
```
sudo mkdir -p /var/www/ghost/example.com
```

# Set directory owner: Replace user with the name of your user
```
sudo chown chandima:chandima /var/www/ghost/example.com
```
# Set the correct permissions
```
sudo chmod 775 /var/www/ghost/example.com
```

# Then navigate into it
```
cd /var/www/ghost/example.com
```

Step 7: Install Ghost
```
ghost install
```

Step 8: Custom SSL

navigate nginx config
```
cd /etc/nginx/sites-available/
```

file list
```
ls
```

edit nginx config
```
sudo nano example.com.conf
```

change to root user
```
sudo su
```

nginx check
```
nginx -t
```

#restart nginx system
```
sudo systemctl restart nginx.service
```

#Install SSL
```
sudo certbot --nginx -d example.com -d www.example.com
```

