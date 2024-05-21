# AWS IAM Setup for DevOps Team

This guide provides step-by-step instructions for setting up AWS Identity and Access Management (IAM) for a DevOps team. The setup includes creating a group, assigning policies, creating a user, and setting up login credentials.

## Steps

### Step 1: Configure AWS CLI

First, ensure that the AWS Command Line Interface (CLI) is configured. This will allow you to run AWS commands from your terminal.

```bash
aws configure

You will be prompted to enter your AWS Access Key, Secret Key, region, and output format. This step is essential for authenticating your AWS CLI with your AWS account.

### Step 2: Create an IAM Group
Next, create a group named DevOps. This group will contain all the IAM users who need specific permissions related to DevOps tasks.

```bash
aws iam create-group --group-name DevOps

### Step 3: Attach Policies to the Group
Attach necessary policies to the DevOps group. These policies will grant the required permissions to the users in the group.

- AmazonS3FullAccess: Grants full access to Amazon S3.
- AmazonEC2ReadOnlyAccess: Grants read-only access to Amazon EC2.
- IAMUserChangePassword: Allows users to change their own passwords.

```bash
aws iam attach-group-policy --group-name DevOps --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam attach-group-policy --group-name DevOps --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
aws iam attach-group-policy --group-name DevOps --policy-arn arn:aws:iam::aws:policy/IAMUserChangePassword

### Step 4: Create a New IAM User
Create a new IAM user named alexis.chance.

```bash
aws iam create-user --user-name alexis.chance

### Step 5: Add the User to the Group
Add the user alexis.chance to the DevOps group.

```bash
aws iam add-user-to-group --user-name alexis.chance --group-name DevOps

### Step 6: Create a Login Profile for the User
Finally, create a login profile for the user alexis.chance. This will set up a password for console access. The --password-reset-required flag forces the user to change their password upon first login.

```bash
aws iam create-login-profile --user-name alexis.chance --password Password123! --password-reset-required

