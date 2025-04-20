# AWS CodePipeline Setup Guide for vProfile Application

### Pipeline Creation

1. Initial Setup

- Navigate to AWS CodePipeline console

- Click "Create pipeline"

- Category

![Image](https://github.com/user-attachments/assets/e660d4a6-8166-4b56-a859-32080c7d60a6)

- Pipeline settings:

    - Pipeline name: vprofile-cicd-pipeline
    
    - Service role: "New service role" (will be created automatically)
    
    - Role name: vprofile-pipeline-service-role
    
    - Artifact store: "Default location" (or select your S3 bucket)

![Image](https://github.com/user-attachments/assets/8bd65570-2cef-42ce-beb2-d8f13996ea34)


### Stage Configuration

![Image](https://github.com/user-attachments/assets/61db3ec1-ce9a-4d52-b0ed-364e20bd7bbf)

![Image](https://github.com/user-attachments/assets/49fad2bb-b1a0-434f-87f8-d5b26bb56d13)

1. Source Stage

- Source provider: Select your repository source (Bitbucket/GitHub/CodeCommit)

- Repository name: Select your vprofile repository

- Branch name: aws-ci (or your target branch)

- Change detection options:

    - For GitHub/Bitbucket: Enable webhooks
    
    - For CodeCommit: Use CloudWatch Events


2. Build Stage

![Image](https://github.com/user-attachments/assets/1a4a42fb-4255-416d-bac1-3b10d368b9bb)

![Image](https://github.com/user-attachments/assets/dd445125-c18e-4f0d-85eb-ba33ff80d68d)

- Build provider: AWS CodeBuild

- Project name: Select your existing vprofile-build project

- Build type: Single build


3. Deploy Stage

![Image](https://github.com/user-attachments/assets/c3bea158-5da6-4ddf-8ee8-fee4f9e6cb1b)

![Image](https://github.com/user-attachments/assets/11c00921-e62d-4991-a6d9-2620335e48fb)

- Deploy provider: AWS Elastic Beanstalk

- Application name: Select your Elastic Beanstalk application

- Environment name: Select your target environment

- Artifact: Select the build artifact from previous stage


### IAM Permissions Setup

Critical Permission Update

After initial pipeline creation:

- Navigate to IAM console

- Go to "Roles" section

- Find the role created for your pipeline (vprofile-pipeline-service-role)

- Click "Attach policies"

- Search for and add: AdministratorAccess-AWSElasticBeanstalk

- Click "Attach policy"

Recommended Permissions

For production environments, consider more granular permissions:

```bash
AWSElasticBeanstalkWebTier

AWSElasticBeanstalkWorkerTier

AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy
```

### Pipeline Execution

![Image](https://github.com/user-attachments/assets/1d054c96-f388-4040-836a-2123e477e1a5)

Initial Run

- Return to CodePipeline console

- Locate your vprofile-cicd-pipeline

- Click "Release change" to manually trigger execution

- Monitor progress through each stage

### Troubleshooting

Common Issues

Deployment failures:

- Check CloudTrail for permission errors

- Verify Elastic Beanstalk environment health

- Confirm artifact was properly generated in build stage

Source stage problems:

- Verify repository webhook configuration

- Check CodeBuild service role permissions

- Confirm source credentials are valid

Build stage failures:

- Review CodeBuild logs

- Verify buildspec file is correct

- Check environment variables


