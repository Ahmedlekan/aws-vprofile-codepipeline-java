# AWS RDS MySQL Setup & Connection Guide

### Database Creation

1. Steps to create MySQL RDS instance:

 - Navigate to AWS RDS service
 
 - Click "Create database"
 
 - Select "Standard create" and choose MySQL engine

![Image](https://github.com/user-attachments/assets/b9dbac58-1b34-428a-bb6d-5485b175823c)

2. Configure instance settings:

 - Availability and durability: Select based on your requirements

![Image](https://github.com/user-attachments/assets/8d6e14d1-058d-45c1-b96a-2ac2e89c0f92)

-  Settings
![Image](https://github.com/user-attachments/assets/bb146127-fcc8-4332-a207-c8ff2a5fc7fe)
 
 - Instance configuration: Choose appropriate instance class

![Image](https://github.com/user-attachments/assets/e2148ab3-8232-4804-9eff-8a680e2c6b1f)
 
 - Storage: Allocate sufficient storage with autoscaling if needed

![Image](https://github.com/user-attachments/assets/acfcdb4a-e84a-4d14-985e-26ce8b2c3465)


3. Set connectivity options:

- Configure VPC and subnet group

![Image](https://github.com/user-attachments/assets/9784fa3c-22d4-40d3-b80b-db942ea8760b)


4. Set database authentication:

  - Choose password authentication
  
  - Set username as admin

  - Create a strong password (save this securely)

  - Databae options

![Image](https://github.com/user-attachments/assets/62605088-1ae3-4ccd-88b0-5fee5c5d79e5)
  

5. Review and create database

⚠️ Important: Save the database endpoint and credentials securely as they will be needed later.


### Security Group Configuration

To allow traffic from your Beanstalk/EC2 instance:

- Navigate to EC2 > Security Groups

- Locate the RDS security group

- Edit inbound rules:

   - Add rule: MySQL/Aurora (port 3306)
   
   - Source: Custom > Select your Beanstalk/EC2 instance security group
   
   - Save rules


### Database Connection Verification

Connect from your EC2 instance to verify the connection:

- Open an SSH client.

- Locate your private key file. The key used to launch this instance is private_key_file

```bash
# Connect to your EC2 instance

chmod 400 "private_key_file"

ssh -i "vprofile-awscicd.pem" ec2-user@<your_instance_ip>
  
# Switch to root
sudo -i
  
# Install MySQL client
dnf search mysql
dnf install mariadb105
  
# Connect to RDS
mysql -h <your_rds_endpoint> -u admin -p
  
# When prompted, enter your RDS password
# Verify connection
USE accounts;
SHOW DATABASES;
EXIT;
```

### Database Initialization

Initialize your database with the schema and initial data:

```bash
  # Download database backup
  wget https://raw.githubusercontent.com/Ahmedlekan/devops-project/main/src/main/resources/db_backup.sql
  
  # Verify download
  ls -la  # Should show db_backup.sql
  
  # Import database
  mysql -h <your_rds_endpoint> -u admin -p < db_backup.sql
  
  # Verify import
  mysql -h <your_rds_endpoint> -u admin -p
  USE accounts;
  SHOW TABLES;
  EXIT;
```

### Troubleshooting

- Connection issues: Verify security group rules and network ACLs

- Authentication errors: Double-check username/password

- Import errors: Ensure the SQL file downloaded completely

### Best Practices for Production

- Always store credentials in AWS Secrets Manager

- Enable automated backups for your RDS instance

- Configure monitoring alerts for database metrics

- Consider using IAM database authentication for better security

