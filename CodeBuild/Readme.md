# AWS CodeBuild Project Setup Guide for CI/CD Pipeline

### Prerequisites

- AWS account with appropriate permissions

- Source code repository (Bitbucket/GitHub/CodeCommit)

- RDS database endpoint and credentials

- IAM roles with CodeBuild permissions


### S3 Bucket Creation

1. Steps:

- Navigate to AWS S3 console

- Click "Create bucket"

2. Configure:

- Bucket name: vprofile-artifacts-[unique-suffix] (must be globally unique)

- Region: Same as your other resources

- Block Public Access: Keep enabled

- Click "Create bucket"


### CodeBuild Project Setup

![Image](https://github.com/user-attachments/assets/ab5171a1-33c5-4127-8279-e9e2d2b017e4)

1. Project Configuration

- Navigate to AWS CodeBuild console

- Click "Create build project"

- Project details:

  - Project name: vprofile-build
  
  - Description: "Build project for vProfile application"
 
  
2. Source Configuration

![Image](https://github.com/user-attachments/assets/ded46477-3f71-446e-8e8f-cb45056c0e53)

- Source provider: Select your repository source (Bitbucket/GitHub/CodeCommit)

- Repository: Select your vprofile repository

- Source version: aws-ci (or your target branch)

- Webhook: Enable if automatic builds on push needed


3. Environment Configuration

Environment image:

![Image](https://github.com/user-attachments/assets/b3401dfe-004d-4595-86ef-020bc4f16194)

- Managed image

- Operating system: Ubuntu

- Runtime: Standard

- Image: aws/codebuild/standard:7.0

- Service role: Create new or select existing

- Privileged: Disable (unless Docker builds needed)

Compute:

- Type: EC2 (or ARM/Linux GPU if needed)

- Size: Small (adjust based on project needs)


4. Buildspec Configuration

- Select "Insert build commands" > "Switch to editor"

- Paste the build specification

```bash
version: 0.2
#env:
  #variables:
     # key: "value"
     # key: "value"
  #parameter-store:
     # key: "value"
     # key: "value"

phases:
  install:
   runtime-versions:
      java: corretto17
  pre_build:
    commands:
      - apt-get update
      - apt-get install -y jq 
      - wget https://archive.apache.org/dist/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz
      - tar xzf apache-maven-3.9.8-bin.tar.gz
      - ln -s apache-maven-3.9.8 maven
      - sed -i 's/jdbc.password=admin123/jdbc.password=*your_rds_passworld/' src/main/resources/application.properties
      - sed -i 's/jdbc.username=admin/jdbc.username=admin/' src/main/resources/application.properties
      - sed -i 's/db01:3306/your_rds_endpoint:3306/' src/main/resources/application.properties
  build:
    commands:
      - mvn install
  post_build:
    commands:
       - mvn package
artifacts:
  files:
     - '**/*'
  base-directory: 'target/vprofile-v2'
```

Security Recommendations:

 - Never hardcode credentials:

    - Use AWS Systems Manager Parameter Store for RDS_PASSWORD
    
    - Reference as ${RDS_PASSWORD} in buildspec

  - Environment variables:

    - Configure sensitive values in CodeBuild environment variables
    
    - Mark them as "Secure" to encrypt them


5. Artifacts Configuration

![Image](https://github.com/user-attachments/assets/878c4056-c0f5-4a1b-8bf6-0483a6b8de63)

- Type: Amazon S3

- Bucket name: Select your created S3 bucket

- Name: vprofile-build-artifacts

- Artifacts packaging: Zip (optional)


6. Logs Configuration

![Image](https://github.com/user-attachments/assets/0cc87fab-7a4b-497f-a6b0-839348e15161)

- CloudWatch Logs: Enable

- Group name: /aws/codebuild/vprofile-build

- Stream name: build-logs

### Verification

![Image](https://github.com/user-attachments/assets/5976daa2-81e8-432a-b1b3-69724db581c9)

Manual Build Test:

- Navigate to your CodeBuild project

- Click "Start build"

- Monitor build logs in real-time

Verify:

- Build completes successfully

- Artifacts are uploaded to S3

- Application properties are correctly modified

### Automated Trigger Test:

- Push a change to your repository

- Verify CodeBuild automatically starts

- Check build status notifications

### Troubleshooting

Common Issues:

- Permission errors:

    - Verify IAM role has S3 write permissions
    
    - Check service role trust policy

- Build failures:

    - Check CloudWatch logs for detailed errors
    
    - Verify network connectivity to RDS

- Artifact upload failures:

    - Confirm S3 bucket exists
    
    - Check bucket policy permissions


