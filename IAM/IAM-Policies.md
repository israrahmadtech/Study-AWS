# AWS IAM Policies

## 1. Introduction

AWS Identity and Access Management (IAM) Policies are documents that define permissions in AWS. They determine **who can perform which actions, on which resources, and under what conditions**.

Policies are one of the most important parts of AWS security because they control access to AWS services and resources.

For example:

- A developer can view files in an S3 bucket but cannot delete them.
- A backend server can access a specific S3 bucket.
- A team member can start and stop EC2 instances but cannot terminate them.
- An administrator can manage IAM users and permissions.

**In simple words:** IAM Policies define what an identity is allowed or denied to do in AWS.

---

## 2. Why Do We Need IAM Policies?

Imagine a software company with three teams:

- Frontend Developers
- Backend Developers
- DevOps Engineers

Each team needs different AWS permissions.

| Team                | Required Permissions                          |
| ------------------- | --------------------------------------------- |
| Frontend Developers | Read frontend files from S3                   |
| Backend Developers  | Read and upload application files to S3       |
| DevOps Engineers    | Manage EC2 instances and deployment resources |

Giving everyone full access to AWS would be dangerous.

IAM Policies solve this problem by allowing administrators to assign only the permissions required by each user, group, or role.

### Main Purposes

- Control access to AWS services.
- Grant specific permissions.
- Deny unauthorized actions.
- Restrict access to specific resources.
- Apply the Principle of Least Privilege.
- Improve AWS account security.

---

## 3. How IAM Policies Work

An IAM Policy is attached to an identity or resource.

For example:

1. An administrator creates an IAM Policy.
2. The policy defines allowed or denied actions.
3. The administrator attaches it to an IAM User, Group, Role, or supported AWS Resource.
4. When someone makes an AWS request, AWS evaluates the applicable policies.
5. AWS allows or denies the request based on the evaluation.

### Simple Example

Suppose an IAM User named `backend-developer` needs to read files from an S3 bucket.

The administrator attaches a policy that allows:

- Listing the specified bucket.
- Reading objects from that bucket.

The user can perform these actions but does not automatically receive permission to delete files or access other buckets.

---

## 4. Understanding IAM Policy Structure

Most IAM permission policies are written in JSON format.

A policy contains one or more statements. Each statement defines a permission.

### Basic JSON Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadAccess",
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

This policy allows the implied IAM identity to list the specified bucket and read its objects.

Let's understand each element.

### 4.1 Version

```json
"Version": "2012-10-17"
```

Defines the version of the IAM policy language.

The recommended version is:

`2012-10-17`

**Important:** This is the policy language version, not the date when the policy was created.

### 4.2 Statement

```json
"Statement": []
```

Contains the permission statements.

A policy can contain:

- One statement.
- Multiple statements.

Each statement defines a permission or restriction.

### 4.3 Sid (Statement ID)

```json
"Sid": "AllowS3ReadAccess"
```

An optional identifier used to describe or distinguish a statement.

It helps administrators understand what a statement does, especially when a policy contains multiple statements.

### 4.4 Effect

```json
"Effect": "Allow"
```

Defines whether an action is allowed or denied.

There are two possible values:

| Effect | Meaning                      |
| ------ | ---------------------------- |
| Allow  | Grants permission            |
| Deny   | Explicitly blocks permission |

Example:

```json
"Effect": "Allow"
```

This allows the specified action.

```json
"Effect": "Deny"
```

This explicitly denies the specified action.

**Important:** An explicit Deny overrides an Allow.

### 4.5 Action

```json
"Action": "s3:GetObject"
```

Defines which AWS actions are allowed or denied.

Examples:

| Action                  | Purpose                       |
| ----------------------- | ----------------------------- |
| `s3:GetObject`          | Read an S3 object             |
| `s3:PutObject`          | Upload an object to S3        |
| `s3:DeleteObject`       | Delete an S3 object           |
| `s3:ListBucket`         | List objects in an S3 bucket  |
| `ec2:StartInstances`    | Start EC2 instances           |
| `ec2:StopInstances`     | Stop EC2 instances            |
| `ec2:DescribeInstances` | View EC2 instance information |

AWS actions usually follow this format:

`service:ActionName`

For example:

- `s3:GetObject`
- `ec2:StartInstances`
- `iam:CreateUser`

### 4.6 Resource

```json
"Resource": "arn:aws:s3:::my-app-bucket/*"
```

Defines which AWS resources the action applies to.

AWS resources are commonly identified through Amazon Resource Names (ARNs).

Example S3 bucket ARN:

```text
arn:aws:s3:::my-app-bucket
```

Example S3 object ARN pattern:

```text
arn:aws:s3:::my-app-bucket/*
```

The `*` wildcard represents matching objects under that bucket.

**Important:** `s3:ListBucket` applies to the bucket ARN, while `s3:GetObject` applies to object ARNs. These are different resource types.

### 4.7 Condition

```json
"Condition": {
  "StringEquals": {
    "aws:RequestedRegion": "us-east-1"
  }
}
```

An optional element that adds conditions to a permission.

It allows AWS to evaluate additional details about a request before granting access.

Examples of possible conditions:

- Allow access only from a specific IP address.
- Allow actions only in a particular AWS Region.
- Allow access only when a specific request condition is satisfied.

Conditions make policies more precise and secure.

### 4.8 Principal

```json
"Principal": {
  "AWS": "arn:aws:iam::123456789012:user/example-user"
}
```

Identifies who is allowed or denied access in a resource-based policy.

For example, an S3 bucket policy can specify which AWS account or IAM identity may access the bucket.

**Important distinction:**

- Identity-based policies do not include a `Principal` element. The identity to which the policy is attached is implied.
- Resource-based policies generally specify a `Principal` to identify the identity receiving access.

---

## 5. Main Types of IAM Policies

IAM supports different policy types. The most important types to understand initially are Identity-based Policies and Resource-based Policies.

### 5.1 Identity-based Policies

Identity-based policies are attached to IAM identities.

These identities include:

- IAM Users
- IAM Groups
- IAM Roles

They define what actions an identity can perform on AWS resources.

Example:

An IAM User receives permission to read files from an S3 bucket.

Identity-based policies are divided into two main categories:

#### A. Managed Policies

Managed policies are standalone policies that can be attached to multiple IAM identities.

There are two types:

**1. AWS Managed Policies**

These policies are created and maintained by AWS.

Examples:

- `AdministratorAccess`
- `ReadOnlyAccess`
- `AmazonS3ReadOnlyAccess`
- `AmazonEC2FullAccess`

Advantages:

- Ready to use.
- Maintained by AWS.
- Can be attached to multiple identities.

**2. Customer Managed Policies**

These policies are created and managed by you inside your AWS account.

Example:

You create a policy named:

`BackendS3ReadOnlyPolicy`

It allows backend developers to read files from a specific S3 bucket.

Advantages:

- Custom permissions.
- Reusable across multiple users, groups, and roles.
- You control updates and maintenance.

#### B. Inline Policies

Inline policies are directly embedded into a single IAM User, Group, or Role.

They have a strict one-to-one relationship with that identity.

Example:

You create an inline policy directly inside an IAM Role to allow a specific application permission.

Important characteristics:

- Directly attached to one identity.
- Not managed as a standalone reusable policy.
- Deleted when the associated identity is deleted.

### Managed vs Inline Policies

| Feature                | Managed Policy      | Inline Policy |
| ---------------------- | ------------------- | ------------- |
| Standalone policy      | Yes                 | No            |
| Reusable               | Yes                 | No            |
| Attached to            | Multiple identities | One identity  |
| Independent management | Yes                 | No            |
| Deleted with identity  | No                  | Yes           |

**Best Practice:** Prefer customer managed policies when you need reusable custom permissions. Use inline policies when you specifically need a one-to-one policy relationship.

### 5.2 Resource-based Policies

Resource-based policies are attached directly to supported AWS resources.

They define:

- Who can access a resource.
- Which actions they can perform.
- Under what conditions access is allowed.

Examples:

- Amazon S3 Bucket Policies.
- Amazon SQS Queue Policies.
- AWS KMS Key Policies.
- IAM Role Trust Policies.

Example:

An S3 bucket policy allows a particular AWS account to read objects from a bucket.

Important characteristics:

- Attached to a resource.
- Specifies a Principal.
- Resource-based policies are inline, not managed policies.
- Can be used to grant cross-account access where supported.

### Identity-based vs Resource-based Policies

| Feature           | Identity-based                 | Resource-based                          |
| ----------------- | ------------------------------ | --------------------------------------- |
| Attached to       | User, Group, Role              | AWS Resource                            |
| Main purpose      | Define what an identity can do | Define who can access a resource        |
| Principal element | Not included                   | Generally included                      |
| Example           | Allow a user to read S3 files  | Allow an account to access an S3 bucket |

---

## 6. Allow vs Deny in IAM Policies

Understanding Allow and Deny is extremely important.

### 6.1 Default Deny

By default, AWS denies requests unless applicable permissions allow them.

For example:

If a user has no permission to access an S3 bucket, AWS denies the request.

This is called an **Implicit Deny**.

### 6.2 Explicit Allow

An explicit Allow is written in a policy.

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

This allows the identity to read objects from the specified bucket.

### 6.3 Explicit Deny

An explicit Deny directly blocks an action.

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

This explicitly denies deleting objects from the specified bucket.

### Most Important Rule

**Explicit Deny always overrides Allow.**

Example:

- Policy A allows a user to read and delete S3 objects.
- Policy B explicitly denies deleting S3 objects.

Final result:

- Read objects: Allowed.
- Delete objects: Denied.

Even though one policy allows deletion, the explicit Deny takes priority.

---

## 7. Understanding ARN (Amazon Resource Name)

ARN stands for **Amazon Resource Name**.

It is a unique identifier used to identify AWS resources.

General format:

```text
arn:partition:service:region:account-id:resource
```

Example:

```text
arn:aws:s3:::my-app-bucket
```

Let's understand its parts:

| Part             | Meaning                                           |
| ---------------- | ------------------------------------------------- |
| `arn`            | Identifies the ARN format                         |
| `aws`            | AWS partition                                     |
| `s3`             | AWS service                                       |
| Empty Region     | S3 bucket ARN does not specify a Region here      |
| Empty Account ID | S3 bucket ARN does not specify an account ID here |
| `my-app-bucket`  | Resource name                                     |

Another example:

```text
arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0
```

This identifies an EC2 instance.

### Why Are ARNs Important?

ARNs help policies specify exactly which AWS resources an action applies to.

For example, instead of granting access to every S3 bucket, you can grant access to one particular bucket.

**Security Tip:** Avoid using `Resource: "*"` when you can specify the exact resources required.

---

## 8. Understanding Wildcards

IAM policies support wildcard characters.

The two common wildcard characters are:

| Wildcard | Meaning                         |
| -------- | ------------------------------- |
| `*`      | Matches zero or more characters |
| `?`      | Matches a single character      |

Example:

```json
"Action": "s3:*"
```

This matches all S3 actions.

Example:

```json
"Resource": "*"
```

This applies to all resources supported by the specified action.

Wildcards can be useful, but careless use can grant excessive permissions.

**Best Practice:** Use specific actions and resource ARNs whenever possible.

---

## 9. Practical IAM Policy Examples

These examples are for learning and understanding policy structure. They have not yet been deployed or tested in an AWS account.

### Example 1: S3 Read-Only Access

A developer can list a bucket and read its objects.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowBucketListing",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-app-bucket"
    },
    {
      "Sid": "AllowObjectReading",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

### Example 2: S3 Upload Access

A backend application can upload files to a specific bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowFileUploads",
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

### Example 3: EC2 Start and Stop Access

A developer can start and stop EC2 instances.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2StartStop",
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:us-east-1:123456789012:instance/*"
    }
  ]
}
```

This example is restricted to EC2 instances in the specified account and Region. Additional permissions may be needed for related console operations.

### Example 4: Deny S3 Object Deletion

A user cannot delete objects from a particular bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyObjectDeletion",
      "Effect": "Deny",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

### Example 5: Allow Access Only From a Specific IP

This policy allows S3 object reading only when the request comes from the specified public IP address.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadFromSpecificIP",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.10/32"
        }
      }
    }
  ]
}
```

The IP address above is an example documentation address. Replace it with the appropriate public IP when implementing a real policy.

---

## 10. How AWS Evaluates IAM Policies

Whenever an IAM User or Role makes a request, AWS evaluates the applicable policies to decide whether the request should be allowed or denied.

The basic process is:

1. AWS authenticates the identity making the request.
2. AWS identifies the request details.
3. AWS identifies the applicable policies.
4. AWS evaluates Allow, Deny, and any applicable restrictions.
5. AWS allows or denies the request.

### Important Evaluation Rules

- By default, requests are denied.
- An applicable explicit Allow can grant permission.
- An explicit Deny overrides an Allow.
- Additional permission controls can limit the permissions granted by other policies.

### Policy Evaluation With Multiple Policy Types

Some AWS policy types act as permission limits rather than granting permissions.

Important examples:

**1. Permissions Boundaries**

A permissions boundary defines the maximum permissions that identity-based policies can grant to an IAM User or Role.

It does not grant permissions by itself.

**2. Service Control Policies (SCPs)**

SCPs are used with AWS Organizations.

They define the maximum available permissions for IAM users and roles in affected member accounts.

They do not grant permissions by themselves.

**3. Session Policies**

Session policies can limit the permissions available to a temporary role or federated session.

They do not grant additional permissions beyond the permissions available to the session.

**Important:** The exact evaluation logic depends on the policy types involved and whether the request is within the same AWS account or across accounts.

For a beginner, remember:

- Policies can grant permissions.
- Some policy types limit permissions.
- Explicit Deny overrides Allow.
- Every required permission must be satisfied by the applicable policy evaluation rules.

---

## 11. IAM Policy vs IAM User vs IAM Group vs IAM Role

These four IAM concepts work together, but they have different responsibilities.

| Concept    | Purpose                                              |
| ---------- | ---------------------------------------------------- |
| IAM User   | Represents an individual identity                    |
| IAM Group  | Organizes IAM Users                                  |
| IAM Role   | Provides temporary permissions to a trusted identity |
| IAM Policy | Defines permissions                                  |

### Real-World Example

Imagine a backend development team.

1. Create an IAM Group named `BackendTeam`.
2. Add backend developers to that group.
3. Create a customer managed policy named `BackendS3Policy`.
4. Allow the required S3 actions in the policy.
5. Attach the policy to `BackendTeam`.

Now the users in that group receive the permissions defined by the policy.

Similarly, an EC2 instance can use an IAM Role with an attached policy to access an S3 bucket without storing long-term AWS access keys on the server.

---

## 12. Creating an IAM Policy in AWS Console

You can create a customer managed policy through the traditional IAM console.

### Steps

1. Sign in to the AWS Management Console.
2. Open the IAM service.
3. Navigate to **Policies**.
4. Click **Create policy**.
5. Select the Visual or JSON editor.
6. Define the required permissions.
7. Review the policy.
8. Enter a policy name.
9. Add an optional description.
10. Create the policy.

Example policy name:

`BackendS3ReadOnlyPolicy`

After creating the policy, you can attach it to an IAM User, Group, or Role.

### Attaching a Managed Policy

1. Open IAM.
2. Select Users, User Groups, or Roles.
3. Open the identity.
4. Navigate to Permissions.
5. Choose Add permissions.
6. Attach the required policy.
7. Review and save the changes.

**Note:** AWS console layouts can change. The exact labels or navigation steps may differ between console experiences, but the underlying IAM policy concepts remain relevant.

---

## 13. AWS CLI Commands for IAM Policies

AWS CLI can also be used to manage IAM policies.

### Create a Customer Managed Policy

```bash
aws iam create-policy \
  --policy-name BackendS3ReadOnlyPolicy \
  --policy-document file://policy.json
```

### Attach a Policy to an IAM User

```bash
aws iam attach-user-policy \
  --user-name backend-user \
  --policy-arn arn:aws:iam::123456789012:policy/BackendS3ReadOnlyPolicy
```

### Attach a Policy to an IAM Group

```bash
aws iam attach-group-policy \
  --group-name BackendTeam \
  --policy-arn arn:aws:iam::123456789012:policy/BackendS3ReadOnlyPolicy
```

### Attach a Policy to an IAM Role

```bash
aws iam attach-role-policy \
  --role-name BackendApplicationRole \
  --policy-arn arn:aws:iam::123456789012:policy/BackendS3ReadOnlyPolicy
```

### List Attached User Policies

```bash
aws iam list-attached-user-policies \
  --user-name backend-user
```

### Detach a Policy From a User

```bash
aws iam detach-user-policy \
  --user-name backend-user \
  --policy-arn arn:aws:iam::123456789012:policy/BackendS3ReadOnlyPolicy
```

**Note:** Replace example names, account IDs, and ARNs with your actual AWS resources. These commands assume that AWS CLI is configured and the caller has the required permissions.

---

## 14. IAM Policy Security Best Practices

IAM Policies directly affect AWS account security. Incorrect permissions can expose resources or allow unauthorized actions.

Follow these best practices:

### 1. Follow the Principle of Least Privilege

Give identities only the permissions they actually need.

Avoid unnecessary permissions.

### 2. Avoid AdministratorAccess

Do not give full administrator permissions to every developer or application.

Use restricted policies based on responsibilities.

### 3. Avoid Unnecessary Wildcards

Avoid broad permissions such as:

```json
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}
```

This can grant extremely broad access.

### 4. Use Specific Resource ARNs

Grant access to specific buckets, instances, or other resources instead of all resources whenever possible.

### 5. Prefer Customer Managed Policies for Reusable Custom Permissions

Create reusable policies for different teams and application roles.

### 6. Review Policies Regularly

Remove unused policies and permissions that are no longer required.

### 7. Use Conditions When Appropriate

Use conditions to add restrictions such as source IP, requested Region, or other supported request details.

### 8. Validate Policies Before Using Them

Review policy syntax and permissions before attaching a policy to an identity.

Useful AWS tools include:

- IAM Policy Simulator.
- IAM Access Analyzer.
- IAM policy validation.

### 9. Do Not Store AWS Credentials in Application Code

For applications running on AWS services such as EC2, prefer IAM Roles and temporary credentials instead of hardcoded access keys.

### 10. Understand Explicit Deny

Always review Deny statements because an explicit Deny can override permissions granted by other applicable policies.

---

## 15. Real-World Example for MERN Stack Developers

Imagine you are building a MERN application with AWS S3 for image and file storage.

Your application has:

- React frontend.
- Node.js and Express backend.
- MongoDB database.
- AWS S3 for file storage.

Your backend needs to upload user profile images to S3.

### Without IAM Policies

You might accidentally give your backend application full AWS permissions.

This creates unnecessary security risks.

### With IAM Policies

You can create a restricted policy that allows only the required S3 actions.

For example:

- `s3:PutObject` — Upload files.
- `s3:GetObject` — Read files, if needed.
- `s3:DeleteObject` — Delete files, only if needed.
- `s3:ListBucket` — List bucket contents, only if needed.

You can then attach this policy to an IAM Role and allow your AWS-hosted backend to use that role.

### Example Architecture

```text
MERN Application
       |
       v
Node.js / Express Backend
       |
       v
EC2 Instance
       |
       v
IAM Role
       |
       v
IAM Policy
       |
       v
Amazon S3 Bucket
```

The IAM Role provides the application's AWS identity, while the IAM Policy defines what that identity is allowed to do.

This is a safer approach than placing long-term AWS access keys directly inside your backend `.env` file.

---

## 16. Important Things to Remember

1. IAM Policies define permissions in AWS.
2. Most IAM permission policies use JSON.
3. A policy contains one or more statements.
4. `Version` defines the policy language version.
5. `Statement` contains permission rules.
6. `Effect` defines Allow or Deny.
7. `Action` defines which AWS operations are permitted or denied.
8. `Resource` defines which resources the actions apply to.
9. `Condition` adds additional restrictions.
10. `Principal` identifies who receives access in resource-based policies.
11. Managed policies can be reused.
12. Inline policies are directly embedded into one identity.
13. Resource-based policies are attached to supported AWS resources.
14. Default access is denied unless applicable permissions allow the request.
15. Explicit Deny overrides Allow.
16. Permissions boundaries and SCPs can restrict available permissions.
17. Always follow the Principle of Least Privilege.

---

## 17. Learning Progress

### IAM Concepts

- [x] IAM Users
- [x] IAM Groups
- [x] IAM Roles
- [x] Least Privilege Principle
- [x] Full Access vs Limited Access
- [x] IAM Policies

### Current Learning Status

- [x] Theoretical learning completed
- [x] Practical implementation pending
- [x] Create custom IAM policies in AWS Console
- [x] Attach policies to IAM Users and Groups
- [ ] Attach policies to IAM Roles
- [ ] Test Allow and Deny permissions
- [ ] Practice least-privilege permissions

**Note:** The examples and commands in this document are for learning purposes. Practical implementation and testing will be completed separately.

---

## 18. Official AWS Documentation

Use these official AWS resources for deeper learning:

1. [Policies and Permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
2. [Overview of JSON Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policies-json)
3. [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
4. [Identity-Based and Resource-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)
5. [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
6. [Policy Evaluation for Requests Within a Single Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic_policy-eval-basics.html)
7. [IAM Policy Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
8. [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
9. [IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)

---

## Conclusion

IAM Policies are the foundation of permission management in AWS.

They allow administrators to control access to AWS services and resources by defining specific permissions for IAM Users, Groups, Roles, and supported resources.

Understanding IAM Policies is essential for developers who want to build secure applications, manage AWS infrastructure, and deploy MERN or Next.js applications on AWS.

**Next Step:** Continue learning the remaining IAM concepts and complete practical IAM exercises before moving to the next AWS service.

---

**Learning Path:** AWS Cloud & DevOps for MERN + Next.js Developers

**Topic:** AWS Identity and Access Management (IAM)

**Document:** IAM Policies

**Status:** Theory Completed | Practical Implementation Pending
