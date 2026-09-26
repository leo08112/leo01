---
title: "Preparing the AWS Environment"
date: 2026-09-06
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
aliases:

---

# 5.1. Preparing the AWS Environment
In this section, we will walk through the steps to set up the basic environment for running a personal project during the internship at AWS Vietnam.

### Prerequisites:
1.  **AWS Account**: An AWS account with permissions to use the services required for the GP, such as IAM, S3, EC2/Lambda, API Gateway, Amazon Transcribe, and Amazon Translate.
2.  **Web Browser**: (Google Chrome, Microsoft Edge, Firefox, or Safari) to access the AWS Management Console and test the web application.
3.  **Development Environment**:
* Visual Studio Code or an equivalent IDE
* Node.js and NPM
* Git for source code management
4. API Testing Tool: Postman or cURL to send requests and test the Backend/API.
5. A device equipped with a microphone to test the system's audio recording and streaming Speech-to-Text functionality.
---

### Step 1: Log in to the Console and Switch Regions
1. Access the [AWS Management Console](https://console.aws.amazon.com/) and log in to your account.
2. In the top-right corner of the navigation bar, select the **Asia Pacific (Singapore) - ap-southeast-1** region. ![Switching Region to Singapore](/PHAMTHO-AWS/picture1/region.jpg)

### Step 2: Check Region

* After switching to Singapore (ap-southeast-1), verify the AWS services intended for use in the project:

1. AWS Service | Purpose in Project
2. IAM | Managing AWS access rights
3. S3 | Storing source code/frontend or static assets
4. EC2 / Lambda | Running the backend
5. API Gateway | Providing APIs/WebSockets
6. Amazon Transcribe | Converting speech to text
7. Amazon Translate | Translating text between languages
8. Expected Outcome

### Step 3: IAM & Access Control Setup

* Create IAM entities, including: Users, Groups, Roles, Policies, and Permissions.
* Security is an integral part of AWS architecture; do not focus solely on application functionality while neglecting access restrictions.

* IAM (Identity and Access Management) is used to control users, roles, and access to AWS services.
![Creating a new user in IAM Users](/PHAMTHO-AWS/picture1/IAM-User.jpg)
* Set up IAM Groups and assign roles and users for accessing specific services.
![Assigning roles and users for accessing specific services](/PHAMTHO-AWS/picture1/IAM-Roles.jpg)

### Step 4: Network & AWS Services Setup

* Set up a VPC dedicated to the project's backend.
![Pre-configured VPC](/PHAMTHO-AWS/picture1/VPC.jpg)

* Configure Subnets and Security Groups. ![Subnets divided based on IPv4 addresses](/PHAMTHO-AWS/picture1/Subnets.jpg)
* Subnets divided based on IPv4 addresses
- The public subnet is configured within the 10.0.1.0/24 range (256 available IPs).
- The same applies to the two private subnets.
![Subnets divided based on IPv4 addresses](/PHAMTHO-AWS/picture1/SUBNET-MAP.jpg)
Only the public subnet can connect to the Internet Gateway.
### Step 5: Create an S3 Bucket

1. Go to the AWS Console, search for S3, and select **Create bucket**.

2. On the **Create bucket** page, enter the following information:

* Bucket name: Enter `dich-2026` (or any unique name).
* AWS Region: Select `ap-southeast-1` (Singapore).

![Subnets divided based on IPv4 addresses](/PHAMTHO-AWS/picture1/s3.jpg)
* Keep ACLs disabled (recommended). Uncheck the **Block all public access** box if you want the JSON files to be publicly downloadable. Acknowledge the warning and click **Create bucket**.