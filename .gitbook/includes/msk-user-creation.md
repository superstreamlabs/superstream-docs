---
title: MSK user creation
---

### Option 1: Create or Update Superstream Role

Be sure you’re signed in to the AWS Console with your default browser, then [**click here**](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?templateURL=https://superstream-aws-cloudformation.s3.eu-central-1.amazonaws.com/iam-role-policy.yaml\&stackName=SuperstreamRoleSetup):

1. Enter required parameters (e.g., NodeGroupRoleArn).
2. Acknowledge IAM resource creation.
3. Click **Create Stack** or **Update Stack** (choose **Update Stack** if the Superstream IAM role already exists).
4. Confirm status: **CREATE\_COMPLETE** or **UPDATE\_COMPLETE**.
5. Click on "Resources," then select "SuperstreamAgentRole" to retrieve the IAM Role ARN. Use this ARN in the Superstream console.

### Option 2: Create or Update Superstream User

Be sure you’re signed in to the AWS Console with your default browser, then [**click here**](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?templateURL=https://superstream-aws-cloudformation.s3.eu-central-1.amazonaws.com/iam-user-policy.yaml\&stackName=SuperstreamUserSetup):

1. Acknowledge IAM resource creation.
2. Click **Create Stack** or **Update Stack** (choose **Update Stack** if the Superstream IAM user already exists).
3. Confirm status: **CREATE\_COMPLETE** or **UPDATE\_COMPLETE (**&#x61;ppears on the left side of the scree&#x6E;**)**.
4. Click on "Resource&#x73;**"** and then click on the created user called "SuperstreamAgentUser".
5. Click on the "Security Credentials" tab, then select "Create access key." Choose "Third-party service" and generate the key. Use this key in the Superstream Console.
