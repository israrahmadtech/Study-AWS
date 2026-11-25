# AWS IAM Roles

## 1. Introduction

AWS Identity and Access Management (IAM) Roles are identities that provide specific permissions to trusted users, applications, or AWS services.

An IAM Role is similar to an IAM User because both can have permissions that control access to AWS resources.

However, there is one major difference:

- IAM Users can have long-term credentials, such as passwords and access keys.
- IAM Roles provide temporary security credentials when they are assumed.

An IAM Role is not permanently associated with one person. Instead, it can be assumed by trusted users, applications, or AWS services.

**In simple words:** An IAM Role provides temporary AWS permissions to an identity that is trusted to use it.

---

## 2. Why Do We Need IAM Roles?

Imagine you build a MERN application and deploy its backend on an Amazon EC2 instance.

Your backend needs to upload images to an Amazon S3 bucket.

One approach would be to store AWS Access Key ID and Secret Access Key inside your backend `.env` file.

However, this creates security risks:

- Credentials can accidentally be exposed.
- Developers must manage and rotate credentials.
- Compromised credentials may be misused.
- Long-term credentials can remain valid until they are disabled or rotated.

IAM Roles solve this problem by providing temporary credentials to trusted applications and services.

### Main Purposes

- Provide temporary AWS credentials.
- Allow applications to access AWS services securely.
- Avoid storing long-term credentials in application code.
- Grant permissions to AWS services.
- Enable cross-account access.
- Delegate permissions to trusted users and applications.
- Follow the Principle of Least Privilege.

---

## 3. How IAM Roles Work

An IAM Role works through a trust relationship and permissions.

The basic process is:

1. An administrator creates an IAM Role.
2. A trust policy defines who or what can assume the role.
3. A permissions policy defines what actions the role can perform.
4. A trusted identity assumes the role.
5. AWS Security Token Service (STS) provides temporary security credentials.
6. The identity uses those temporary credentials to access permitted AWS resources.

### Simple Example

Suppose a Node.js backend runs on an EC2 instance and needs access to an S3 bucket.

The process:

1. Create an IAM Role named `BackendS3Role`.
2. Configure its trust policy to trust the EC2 service.
3. Attach a permissions policy allowing specific S3 actions.
4. Attach the role to the EC2 instance.
5. The backend application obtains temporary credentials through the EC2 role.
6. The backend accesses the permitted S3 resources.

The application does not need manually stored long-term AWS access keys.

---

## 4. Understanding the Two Important Policies of IAM Roles

This is one of the most important concepts in IAM Roles.

An IAM Role involves two different policy responsibilities:

1. Trust Policy
2. Permissions Policy

They have different purposes.

### 4.1 Trust Policy

A Trust Policy defines **who is allowed to assume an IAM Role**.

In other words, it defines which identities or services AWS trusts to use that role.

Examples:

- Amazon EC2 service.
- AWS Lambda service.
- An IAM User.
- Another IAM Role.
- An AWS account.

Example Trust Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Let's understand its important elements:

| Element     | Purpose                                    |
| ----------- | ------------------------------------------ |
| `Version`   | Defines the policy language version        |
| `Statement` | Contains the trust rules                   |
| `Effect`    | Allows or denies the trust action          |
| `Principal` | Identifies the trusted entity              |
| `Service`   | Specifies the AWS service                  |
| `Action`    | Defines the action used to assume the role |

In this example:

- `ec2.amazonaws.com` represents the EC2 service.
- `sts:AssumeRole` allows the trusted service to assume the role.

**Result:** Amazon EC2 is trusted to assume this role.

### 4.2 Permissions Policy

A Permissions Policy defines **what actions an IAM Role can perform after it is assumed**.

For example, an EC2 instance might need permission to read files from a specific S3 bucket.

Example Permissions Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::my-app-bucket",
        "arn:aws:s3:::my-app-bucket/*"
      ]
    }
  ]
}
```

This policy allows the role to:

- List the specified S3 bucket.
- Read objects from that bucket.

It does not automatically allow uploading or deleting objects.

### Trust Policy vs Permissions Policy

| Feature           | Trust Policy                    | Permissions Policy           |
| ----------------- | ------------------------------- | ---------------------------- |
| Main purpose      | Defines who can assume the role | Defines what the role can do |
| Important element | `Principal`                     | `Action` and `Resource`      |
| Main action       | `sts:AssumeRole`                | Service-specific actions     |
| Example           | Trust EC2 service               | Allow S3 read access         |
| Attached to       | IAM Role                        | IAM Role                     |

**Remember:**

- Trust Policy = Who can use the role?
- Permissions Policy = What can the role do?

Both are important for an IAM Role to work as intended.

---

## 5. What Is AWS STS?

STS stands for **AWS Security Token Service**.

AWS STS provides temporary security credentials to trusted identities.

These credentials allow users, applications, or AWS services to access AWS resources according to the permissions of an assumed role.

### Temporary Security Credentials

Temporary credentials generally contain:

1. Access Key ID
2. Secret Access Key
3. Session Token

Unlike long-term IAM User credentials, temporary credentials have a limited validity period.

### How STS Works

```text
Trusted Identity
      |
      v
Assume IAM Role
      |
      v
AWS Security Token Service
      |
      v
Temporary Security Credentials
      |
      v
Access AWS Resources
```

### Important STS Operations

| Operation                   | Purpose                                               |
| --------------------------- | ----------------------------------------------------- |
| `AssumeRole`                | Obtain credentials for an IAM Role                    |
| `AssumeRoleWithSAML`        | Assume a role using SAML federation                   |
| `AssumeRoleWithWebIdentity` | Assume a role using a supported web identity provider |
| `GetSessionToken`           | Obtain temporary credentials for an IAM User session  |

**Important:** The exact STS operation depends on the type of identity and authentication method being used.

---

## 6. Types and Common Use Cases of IAM Roles

IAM Roles are used in several AWS scenarios.

### 6.1 Service Roles

Service Roles allow AWS services to assume a role and perform specific tasks.

Examples:

- EC2 accessing S3.
- Lambda accessing DynamoDB.
- AWS services performing actions on your behalf.

Example:

An EC2 instance uses an IAM Role to access a specific S3 bucket.

### 6.2 EC2 Instance Roles

EC2 Instance Roles provide permissions to applications running on Amazon EC2 instances.

Example:

A Node.js backend deployed on EC2 needs to upload images to S3.

Instead of storing AWS credentials in `.env`, you attach an IAM Role to the EC2 instance.

The application can then obtain temporary credentials through the EC2 instance metadata service.

**Important:** EC2 uses an Instance Profile as a container for an IAM Role.

When you create and attach a role through the AWS Console, AWS generally manages the instance profile automatically.

When using the AWS CLI or API, creating and managing the instance profile can be a separate step.

### 6.3 Lambda Execution Roles

An AWS Lambda Execution Role defines what an AWS Lambda function can access.

Example:

A Lambda function needs to read data from DynamoDB.

You create a role that:

- Trusts the Lambda service.
- Allows the required DynamoDB actions.

AWS Lambda assumes the role when executing the function.

### 6.4 Cross-Account Roles

Cross-account roles allow trusted identities from one AWS account to access resources in another AWS account.

Example:

Imagine two AWS accounts:

- Account A: Development Account.
- Account B: Production Account.

A developer working in Account A needs limited access to resources in Account B.

Instead of creating another IAM User in Account B, you can configure a role in Account B that trusts the appropriate principal from Account A.

The trusted principal can assume the role and receive temporary credentials.

### 6.5 Federated Access Roles

Federated access allows users authenticated through an external identity provider to access AWS using IAM Roles.

Examples include identity providers using:

- SAML 2.0.
- OpenID Connect (OIDC).

This approach is commonly used for workforce identity and application authentication integrations.

---

## 7. What Is an Instance Profile?

An Instance Profile is a container used by Amazon EC2 to associate an IAM Role with an EC2 instance.

It allows the EC2 instance to make the role's temporary credentials available to applications running on that instance.

### How It Works

```text
IAM Role
   |
   v
Instance Profile
   |
   v
EC2 Instance
   |
   v
Application
   |
   v
Temporary Credentials
```

### Important Points

- An instance profile contains one IAM Role.
- An IAM Role can be included in multiple instance profiles.
- When using the IAM Console, AWS generally creates and manages the instance profile automatically.
- When using the AWS CLI or API, instance profile management may require separate commands.

**Remember:** An IAM Role and an Instance Profile are not the same thing.

The Role defines the identity and permissions. The Instance Profile helps associate that role with an EC2 instance.

---

## 8. IAM Role vs IAM User

Both IAM Users and IAM Roles represent identities in AWS, but their purposes are different.

| Feature      | IAM User                                       | IAM Role                                 |
| ------------ | ---------------------------------------------- | ---------------------------------------- |
| Identity     | AWS identity                                   | AWS identity                             |
| Main purpose | Long-term identity for a person or application | Delegated or temporary access            |
| Password     | Can have a console password                    | No standard console password             |
| Access Keys  | Can have long-term access keys                 | Uses temporary credentials when assumed  |
| Credentials  | Can be long-term                               | Temporary role-session credentials       |
| Association  | Often associated with a specific user          | Can be assumed by trusted identities     |
| Common use   | Individual IAM access                          | Applications, services, delegated access |

### Example

**IAM User:**

A developer needs an individual AWS identity to access the AWS Console.

**IAM Role:**

A backend application running on EC2 needs permission to upload images to S3.

---

## 9. IAM Role vs IAM Policy

These concepts work together, but they have different responsibilities.

| Feature            | IAM Role                                    | IAM Policy                                                   |
| ------------------ | ------------------------------------------- | ------------------------------------------------------------ |
| Purpose            | Provides an assumable identity              | Defines permissions                                          |
| Credentials        | Provides temporary credentials when assumed | Does not provide credentials                                 |
| Trust relationship | Has a trust policy                          | Ordinary identity-based policy does not establish role trust |
| Permissions        | Receives permissions through policies       | Defines allowed or denied actions                            |
| Example            | BackendS3Role                               | BackendS3ReadOnlyPolicy                                      |

### Real-World Example

1. Create an IAM Role named `BackendS3Role`.
2. Configure its trust policy to trust EC2.
3. Create a policy named `BackendS3ReadOnlyPolicy`.
4. Attach the permissions policy to the role.
5. Attach the role to an EC2 instance.

Now the application running on that instance can use the role's temporary credentials to perform the allowed S3 actions.

---

## 10. Creating an IAM Role in AWS Console

You can create an IAM Role through the AWS Management Console.

### Example: Create an EC2 Role for S3 Access

#### Step 1: Open IAM

1. Sign in to the AWS Management Console.
2. Open the IAM service.
3. Navigate to **Roles**.
4. Click **Create role**.

#### Step 2: Select Trusted Entity

1. Select AWS Service.
2. Choose EC2 as the use case.
3. Continue to the permissions step.

This configures EC2 as the trusted service.

#### Step 3: Add Permissions

1. Search for the required S3 policy.
2. Select an appropriate policy.
3. For production applications, prefer a custom least-privilege policy over broad permissions.

For example:

`BackendS3ReadOnlyPolicy`

#### Step 4: Name the Role

Example:

`BackendS3Role`

Add an optional description explaining its purpose.

#### Step 5: Review and Create

Review:

- Trusted entity.
- Permissions policies.
- Role name.
- Description.

Create the role.

#### Step 6: Attach the Role to EC2

1. Open the EC2 service.
2. Select your EC2 instance.
3. Open the instance actions or security settings.
4. Find the option to modify its IAM role.
5. Select `BackendS3Role`.
6. Save the changes.

Your EC2 instance can now use the role's temporary credentials.

**Note:** AWS Console layouts can change, so the exact labels and navigation steps may differ.

---

## 11. AWS CLI Commands for IAM Roles

AWS CLI can also be used to create and manage IAM Roles.

### Step 1: Create a Trust Policy File

Create a file named:

`trust-policy.json`

Add the following JSON:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Step 2: Create the IAM Role

```bash
aws iam create-role \
  --role-name BackendS3Role \
  --assume-role-policy-document file://trust-policy.json
```

### Step 3: Attach a Permissions Policy

```bash
aws iam attach-role-policy \
  --role-name BackendS3Role \
  --policy-arn arn:aws:iam::123456789012:policy/BackendS3ReadOnlyPolicy
```

Replace the example account ID and policy ARN with your actual values.

### Step 4: Create an Instance Profile

```bash
aws iam create-instance-profile \
  --instance-profile-name BackendS3InstanceProfile
```

### Step 5: Add the Role to the Instance Profile

```bash
aws iam add-role-to-instance-profile \
  --instance-profile-name BackendS3InstanceProfile \
  --role-name BackendS3Role
```

### Step 6: Associate the Instance Profile With EC2

```bash
aws ec2 associate-iam-instance-profile \
  --instance-id i-1234567890abcdef0 \
  --iam-instance-profile Name=BackendS3InstanceProfile
```

The example instance ID must be replaced with your actual EC2 instance ID.

**Important:** These commands are for learning. They assume AWS CLI is configured and the caller has the required permissions. When using the CLI, instance profile creation and association are separate steps.

---

## 12. The Difference Between AssumeRole and PassRole

These two IAM concepts are often confused.

### AssumeRole

`sts:AssumeRole` is an AWS STS action used by a trusted principal to obtain temporary credentials for a role.

Example:

An IAM User assumes a role to access resources in another AWS account.

### PassRole

`iam:PassRole` is a permission that allows a principal to pass an IAM Role to an AWS service when configuring or creating a resource.

Example:

A developer launches an EC2 instance and assigns an IAM Role to it.

The developer needs permission to pass that role to EC2.

### Comparison

| Feature | AssumeRole                          | PassRole                                          |
| ------- | ----------------------------------- | ------------------------------------------------- |
| Action  | `sts:AssumeRole`                    | `iam:PassRole`                                    |
| Purpose | Obtain temporary role credentials   | Allow a role to be passed to an AWS service       |
| Used by | Trusted principal                   | Person or application configuring an AWS resource |
| Example | User assumes another account's role | Developer assigns a role to EC2                   |

**Important:** PassRole does not itself provide temporary credentials or allow the caller to assume the role.

---

## 13. Real-World Example for MERN Stack Developers

Imagine you are deploying a MERN application on AWS.

Your application includes:

- React frontend.
- Node.js and Express backend.
- MongoDB database.
- Amazon EC2 for backend hosting.
- Amazon S3 for image storage.

Your backend needs to upload user profile images to S3.

### Without IAM Roles

You might store these credentials inside your backend `.env` file:

```env
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
```

This approach requires you to manage long-term credentials.

If these credentials are exposed, someone could misuse the permissions associated with them.

### With IAM Roles

Instead:

1. Create an IAM Role named `MERNBackendRole`.
2. Configure its trust policy to trust EC2.
3. Attach a permissions policy allowing only the required S3 actions.
4. Attach the role to your EC2 instance.
5. Configure your Node.js application to use the AWS SDK's default credential provider chain.

The SDK can automatically retrieve temporary credentials from the EC2 environment.

### Example Architecture

```text
React Frontend
      |
      v
Node.js / Express Backend
      |
      v
Amazon EC2
      |
      v
IAM Role
      |
      v
IAM Permissions Policy
      |
      v
Amazon S3
```

This approach avoids hardcoding long-term AWS access keys into the application.

---

## 14. IAM Role Security Best Practices

IAM Roles are important for secure AWS infrastructure.

Follow these best practices:

### 1. Follow the Principle of Least Privilege

Give roles only the permissions required for their specific purpose.

### 2. Use Temporary Credentials

Prefer IAM Roles and temporary credentials over long-term access keys for applications running on AWS.

### 3. Configure Trust Policies Carefully

Only trusted users, accounts, applications, and AWS services should be allowed to assume a role.

### 4. Avoid Broad Permissions

Avoid attaching `AdministratorAccess` to application roles unless there is a clearly justified requirement.

### 5. Use Specific Resource ARNs

Limit role permissions to the exact resources required.

### 6. Review IAM Roles Regularly

Remove unused roles and permissions.

### 7. Protect Cross-Account Access

Carefully configure trusted accounts and principals.

For third-party cross-account access, use an external ID when appropriate.

### 8. Restrict iam\:PassRole

Allow users to pass only the specific roles they need to assign to AWS services.

### 9. Monitor Role Usage

Use AWS CloudTrail to review role assumptions and related API activity.

### 10. Separate Roles for Different Applications

Avoid using one overly powerful role for unrelated applications.

Separate roles provide better permission control.

---

## 15. Important Things to Remember

1. IAM Roles are AWS identities with specific permissions.
2. Roles do not have standard long-term passwords or access keys.
3. Roles provide temporary security credentials when assumed.
4. AWS STS is responsible for issuing temporary credentials.
5. Trust Policies define who can assume a role.
6. Permissions Policies define what the role can do.
7. EC2 applications can use IAM Roles to access AWS services.
8. Lambda functions use Execution Roles.
9. Cross-account roles allow trusted access between AWS accounts.
10. An EC2 Instance Profile associates an IAM Role with an EC2 instance.
11. `sts:AssumeRole` is different from `iam:PassRole`.
12. Prefer temporary credentials over hardcoded access keys.
13. Always follow the Principle of Least Privilege.

---

## 16. Learning Progress

### IAM Concepts

- [x] IAM Users
- [x] IAM Groups
- [x] IAM Roles
- [x] Least Privilege Principle
- [x] Full Access vs Limited Access
- [x] IAM Policies

### IAM Roles Learning Status

- [x] Understand IAM Roles.
- [x] Understand Trust Policies.
- [x] Understand Permissions Policies.
- [x] Understand AWS STS.
- [x] Understand Temporary Security Credentials.
- [x] Understand EC2 Instance Roles.
- [x] Understand Instance Profiles.
- [x] Understand Lambda Execution Roles.
- [x] Understand Cross-Account Roles.
- [x] Understand AssumeRole vs PassRole.
- [x] Create an IAM Role in AWS Console.
- [ ] Attach a permissions policy.
- [ ] Attach a role to an EC2 instance.
- [ ] Test S3 access using temporary credentials.

**Note:** The examples and commands in this document are for learning purposes. Practical implementation and testing will be completed separately.

---

## 17. Official AWS Documentation

Use these official AWS resources for deeper learning:

1. [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
2. [How IAM Roles Work](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)
3. [Trust Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_update-role-trust-policy.html)
4. [Using IAM Roles for EC2 Applications](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html)
5. [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
6. [Using IAM Roles With AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)
7. [Cross-Account Resource Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies-cross-account-resource-access.html)
8. [AWS STS AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)
9. [Secure Access Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/securing_access-keys.html)

---

## Conclusion

IAM Roles are an essential part of AWS security and access management.

They allow trusted users, applications, and AWS services to obtain temporary credentials and access AWS resources without requiring long-term credentials to be stored in application code.

For MERN and Next.js developers, understanding IAM Roles is especially important when deploying applications on EC2, Lambda, and other AWS services.

**Next Step:** Complete practical IAM exercises, then continue learning the next AWS service.

---

**Learning Path:** AWS Cloud & DevOps for MERN + Next.js Developers

**Topic:** AWS Identity and Access Management (IAM)

**Document:** IAM Roles

**Status:** Theory Completed | Practical Implementation Pending
