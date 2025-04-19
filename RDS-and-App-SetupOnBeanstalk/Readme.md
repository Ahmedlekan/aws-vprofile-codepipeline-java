
1. Create a database

Engine options

Mysql


Availability and durability

Settings

Instance configuration

Storage


Connectivity


Database authentication


Additional configuration


Make sure to copy your password and save it. 



2. Security Groups

Edit rds security group to allow traffic from beanstalk instance.





3. Confirm your database is connected to your ec2 instance

1. Connect to your local ec2 instance ssh to your local machine.

Open an SSH client.

Locate your private key file. The key used to launch this instance is private_key_file

Run this command, if necessary, to ensure your key is not publicly viewable.

chmod 400 "private_key_file"

Connect to your instance using its Public DNS:
ssh -i "vprofile-awscicd.pem" ec2-user@your_instance_ip

Login to the root user with sudo -i

Run this command

dnf search mysql
dnf install mariadb105
mysql -h your_rds_endpoint -u admin -p
enter your password
USE accounts;
show databases;
exit
wget https://raw.githubusercontent.com/Ahmedlekan/devops-project/refs/heads/main/src/main/resources/db_backup.sql
ls -la
you should see "db_backup.sql"
mysql -h your_rds_endpoint -u admin -p < db_backup.sql
enter your password
USE accounts;
show tables;