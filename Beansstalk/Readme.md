# AWS Elastic Beanstalk Environment Setup Guide

### Environment Configuration

Basic Setup

  - Navigate to AWS Elastic Beanstalk console
  
  - Click "Create Application"


Environment tier:

![Image](https://github.com/user-attachments/assets/32818b08-38e0-413f-858e-d12e91bbdf4c)


Application information:

![Image](https://github.com/user-attachments/assets/f5d9bcbf-cca9-4ab2-a491-8c925a5c1b41)
  

Environment information:

![Image](https://github.com/user-attachments/assets/8dfad301-530e-466d-b1dc-5029805714ca)


Platform:

![Image](https://github.com/user-attachments/assets/2228dc04-83ab-4c51-adb9-b92ba07515b6)

- Presets: "Custom Configuration"


### Service Access Setup

Service Role Configuration

![Image](https://github.com/user-attachments/assets/b6d3f268-9d2d-4003-a01b-220a85762726)

Permissions:

  - Create vprofile-beanstalkrole and attach required managed policies:
```bash
AdministratorAccess-AWSElasticBeanstalk
AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy
AWSElasticBeanstalkWebTier
AWSElasticBeanstalkRoleSNS
```
  - Attach it as shown in the image above


### Networking

VPC Configuration

  - Select your existing VPC
  
  - Or use the default VPC

### Configure instance traffic and scaling

![Image](https://github.com/user-attachments/assets/d93875fa-ab69-4dfd-9110-af9789e76fe5)

![Image](https://github.com/user-attachments/assets/4e017ba1-1884-4be8-ae7e-5e088ac9bd28)

![Image](https://github.com/user-attachments/assets/e70352e2-8f43-4343-85de-8b0ad679f5c7)

![Image](https://github.com/user-attachments/assets/54a18068-4590-40ff-98d2-6ba2c43740a5)

![Image](https://github.com/user-attachments/assets/1eb73601-f012-49da-ad80-2383e20ea13b)

![Image](https://github.com/user-attachments/assets/cd8438f5-8133-42d9-ad3b-4555aee20335)

![Image](https://github.com/user-attachments/assets/339a5617-d3ae-40fc-affc-70164a4b90fd)

- Enable Session Stickiness -> On the process, click on action to enable session stickiness

![Image](https://github.com/user-attachments/assets/b3be87df-d95e-4ddb-8292-73f94e1761e4) 

![Image](https://github.com/user-attachments/assets/2feed6da-37c6-4900-9f8c-6cc8589e594b)

### Updates & Maintenance

Rolling updates and deployments

![Image](https://github.com/user-attachments/assets/bde597bd-5f7d-4fd6-879e-c8b81127437e)

-Review and Submit


### Post-Setup Steps

  - Verify health status shows "OK"
  
  - Access application via provided URL








5. Configure updates
