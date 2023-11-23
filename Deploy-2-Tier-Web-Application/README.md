This folder contains the reference code for deploying 2-tier web application in AWS using EC2 and RDS.
Instructions to create the setup are provided in the project documentation.


Commands used in this tutorial:

**Install Apache web server and PHP packages**
```
#change user to root
sudo su

#install required packages
dnf update -y
dnf install -y httpd php php-mysqli mariadb105

#start webserver service
systemctl start httpd
systemctl enable httpd
```

**Install mqsql client in the ec2 instance**
```
# install pip (Amazon Linux 2023 does not have one by default)
dnf install -y pip

# install dependencies
dnf install -y mariadb105-devel gcc python3-devel

# install mysqlclient
pip install mysqlclient
```

**Connect to MySQL (MariaDB) and query data**
```
#Connect to database instance. Replace database endpoint with your database endpoint
mysql -h <database endpoint> -u admin –p

#Connect to database and query data

MySQL [(none)]> connect corp 
MySQL [corp]> select * from EMPLOYEES;
```

