---
title: MSK user creation
---

### Option 1: Create or Update Superstream Role

Be sure you’re signed in to the AWS Console with your default browser, then [**click here**](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?templateURL=https://superstream-aws-cloudformation.s3.eu-central-1.amazonaws.com/iam-role-policy.yaml\&stackName=SuperstreamRoleSetup) to:

1. Enter required parameters (e.g., NodeGroupRoleArn).
2. Acknowledge IAM resource creation.
3. Click **Create Stack** or **Update Stack** (choose **Update Stack** if the Superstream IAM role already exists).
4. Confirm status: **CREATE\_COMPLETE** or **UPDATE\_COMPLETE**.
5. Click on **Outputs** to get IAM Role details:

<figure><img src="../assets/image (9).png" alt=""><figcaption></figcaption></figure>

### Option 2: Create or Update Superstream User

Be sure you’re signed in to the AWS Console with your default browser, then [**click here**](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?templateURL=https://superstream-aws-cloudformation.s3.eu-central-1.amazonaws.com/iam-user-policy.yaml\&stackName=SuperstreamUserSetup) to:

1. Acknowledge IAM resource creation.
2. Click **Create Stack** or **Update Stack** (choose **Update Stack** if the Superstream IAM user already exists).
3. Confirm status: **CREATE\_COMPLETE** or **UPDATE\_COMPLETE**.
4. Click on **Outputs** to get the programmatic user details.

<figure><img src="../assets/image (10).png" alt=""><figcaption></figcaption></figure>

5. Create a new Access secret key for the user and use it in SSM Console to connect the new cluster.
