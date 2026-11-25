# AWS Principle of Least Privilege

## 1. Introduction

The **Principle of Least Privilege (PoLP)** is an important security principle in AWS and other computing environments.

It means giving a user, application, service, or system only the minimum permissions required to perform its specific tasks.

These permissions should be limited to:

- Only the necessary actions.
- Only the required resources.
- Only the required period of time.

**In simple words:** Give every user or application only the access it needs — nothing extra.

### Example

Imagine you have a MERN application hosted on Amazon EC2.

Your backend needs to upload images to an Amazon S3 bucket.

It only needs permission to upload files.

You should not give it full access to:

- Delete all S3 buckets.
- Create new AWS users.
- Manage EC2 instances.
- Access unrelated AWS services.

The backend should receive only the permissions required to upload files to its specific S3 bucket.

This is the Principle of Least Privilege.

---

## 2. Why Is Least Privilege Important?

Giving unnecessary permissions creates security risks.

If an account or application is compromised, excessive permissions can allow an attacker to perform actions beyond the application's actual requirements.

### Main Benefits

1. **Improved Security:** Reduces unnecessary access to AWS resources.
2. **Reduced Attack Surface:** Limits the number of actions an attacker can perform.
3. **Limited Damage:** Reduces the potential impact of compromised credentials.
4. **Better Access Control:** Ensures users and applications access only required resources.
5. **Prevents Accidental Changes:** Reduces the possibility of accidental deletion or modification.
6. **Easier Auditing:** Makes it easier to understand why a user or application has particular permissions.
7. **Better Compliance:** Supports security and access-control requirements.

---

## 3. Full Access vs Limited Access

Understanding the difference between full access and limited access is essential.

### 3.1 Full Access

Full Access means allowing broad permissions over a service or resource.

For example:

An application has full access to Amazon S3.

It may be able to:

- Create buckets.
- Delete buckets.
- Upload objects.
- Download objects.
- Delete objects.
- Change bucket configurations.

If the application only needs to upload images, these additional permissions are unnecessary.

### 3.2 Limited Access

Limited Access means granting only the permissions required for a particular task.

For example:

An application needs to upload images to one specific S3 bucket.

It receives permission to upload objects only to that bucket.

It does not receive permission to delete buckets or manage unrelated AWS resources.

### Comparison

| Feature | Full Access | Limited Access |
|---|---|---|
| Permissions | Broad permissions | Specific permissions |
| Resource access | May include many resources | Only required resources |
| Security risk | Higher if compromised | Reduced potential impact |
| Flexibility | More actions available | Only required actions |
| Best use | Carefully controlled administrative tasks | Normal users and applications |

**Remember:** Limited Access should be the default approach. Full Access should only be granted when genuinely necessary and properly controlled.

---

## 4. How Does Least Privilege Work in AWS IAM?

AWS IAM provides several features that help implement the Principle of Least Privilege.

### 4.1 IAM Users

An IAM User should receive only the permissions required for their responsibilities.

Example:

A developer needs to view information about EC2 instances.

Instead of giving the developer `AdministratorAccess`, grant only the required EC2 viewing permissions.

### 4.2 IAM Groups

IAM Groups help manage permissions for multiple users with similar responsibilities.

Example:

A development team has five developers.

You create a group named:

`Developers`

You attach a policy that provides the required development permissions.

All users in the group receive those permissions.

Avoid giving the entire group unnecessary administrative access.

### 4.3 IAM Roles

IAM Roles provide temporary credentials to trusted identities.

Example:

A Node.js backend running on EC2 needs to upload images to S3.

Create an IAM Role that:

- Trusts the EC2 service.
- Allows the required S3 upload action.
- Restricts access to the required bucket or folder.

This prevents the application from needing unnecessary long-term credentials.

### 4.4 IAM Policies

IAM Policies define which actions are allowed or denied on specific AWS resources.

For example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-app-bucket/uploads/*"
    }
  ]
}
```

This policy allows uploading objects into the `uploads/` prefix of the specified bucket.

It does not grant permission to delete objects or manage the bucket.

**Important:** This is an illustrative upload policy. Additional permissions may be needed for other operations, such as listing a bucket or using particular encryption configurations.

---

## 5. Understanding Over-Permissioning

Over-permissioning means giving an identity more permissions than it actually needs.

It is one of the common IAM security mistakes.

### Example of Over-Permissioning

Consider this policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

Let's understand it:

| Element | Meaning |
|---|---|
| `Effect: Allow` | Allows the specified actions |
| `Action: "*"` | Allows all actions covered by the policy |
| `Resource: "*"` | Applies to all resources supported by those actions |

This policy grants extremely broad permissions.

If attached to an application role, it could allow the application to perform many actions across AWS services.

This is generally dangerous for normal application workloads.

### Better Approach

Instead of allowing every action on every resource, specify the exact permissions required.

For example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-app-bucket/uploads/*"
    }
  ]
}
```

Now the policy has a much narrower scope.

**Remember:** Avoid using `Action: "*"` and `Resource: "*"` unless there is a specific and justified requirement.

---

## 6. Real-World AWS Examples

Let's understand how Least Privilege is applied in different situations.

### Example 1: Developer Access

A frontend developer needs to view information about an EC2 instance.

Required access:

- View EC2 instance information.

Unnecessary access:

- Delete EC2 instances.
- Modify IAM policies.
- Create IAM Users.
- Manage billing.

The developer should receive only the required viewing permissions.

### Example 2: Backend S3 Upload

A backend application needs to upload profile images to S3.

Required access:

- `s3:PutObject` on the required object path.

Potentially unnecessary access:

- `s3:DeleteBucket`
- `s3:CreateBucket`
- `s3:DeleteObject`
- `s3:PutBucketPolicy`

Only add additional actions when the application actually requires them.

### Example 3: Database Access

An application needs to read customer records from a database.

Its permissions should be limited to the required database operations and data.

It should not automatically receive permission to:

- Delete the entire database.
- Modify unrelated databases.
- Manage database infrastructure.

Database permissions should also be controlled at the database level, not only through AWS IAM.

### Example 4: CI/CD Pipeline

A GitHub Actions workflow deploys an application to AWS.

The deployment role should have only the permissions required for that deployment.

For example:

- Upload build files to a specific S3 bucket.
- Invalidate a specific CloudFront distribution, if required.

It should not automatically receive full AWS administrator access.

---

## 7. Implementing Least Privilege with IAM Policies

IAM Policies are one of the main ways to implement Least Privilege.

A policy can restrict permissions using:

1. Specific Actions.
2. Specific Resources.
3. Conditions.

### 7.1 Restrict Actions

Instead of:

```json
"Action": "s3:*"
```

Use only the required action:

```json
"Action": "s3:PutObject"
```

### 7.2 Restrict Resources

Instead of:

```json
"Resource": "*"
```

Specify the required bucket and object path:

```json
"Resource": "arn:aws:s3:::my-app-bucket/uploads/*"
```

### 7.3 Use Conditions

Conditions can further restrict when or how a permission can be used.

For example, a policy can restrict access based on factors such as:

- Source IP address.
- Requested region.
- Resource tags.
- Encryption requirements.

Example condition requiring server-side encryption with a particular S3 encryption header:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-app-bucket/uploads/*",
      "Condition": {
        "StringEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    }
  ]
}
```

This is an illustrative example. The condition must match the way the application sends its S3 upload requests.

---

## 8. Understanding AdministratorAccess

AWS provides a managed policy called:

`AdministratorAccess`

It grants broad administrative permissions across AWS services.

### Why Is It Risky?

Imagine you attach `AdministratorAccess` to a Node.js backend role.

If an attacker compromises the backend or its credentials, the attacker may gain access to a much wider range of AWS operations than the application actually needs.

This can increase the potential damage.

### When Should It Be Used?

Administrative access may be necessary for certain trusted administrative responsibilities.

However:

- Do not attach it to application roles by default.
- Avoid giving it to every developer.
- Protect administrative identities with MFA.
- Review who has administrative access.
- Use separate, controlled administrative access when needed.

**Important:** `AdministratorAccess` is not automatically unsafe in every situation. The risk comes from granting broad permissions to identities that do not need them or are not properly protected.

---

## 9. IAM Features That Help Enforce Least Privilege

AWS provides additional IAM and organization-level controls.

### 9.1 Permissions Boundaries

A Permissions Boundary defines the maximum permissions that an IAM User or Role can receive through identity-based policies.

It does not grant permissions by itself.

Think of it as a permissions ceiling.

Example:

A developer role has a policy allowing access to EC2 and S3.

Its permissions boundary allows only a smaller set of EC2 and S3 actions.

The role cannot exceed the permissions allowed by that boundary.

### 9.2 Service Control Policies (SCPs)

Service Control Policies are used with AWS Organizations.

They define the maximum available permissions for accounts or organizational units within an organization.

An SCP does not grant permissions to users or roles.

Instead, it places guardrails on what identities in affected accounts can do.

### 9.3 Session Policies

Session Policies can restrict permissions for a particular role session.

They do not grant additional permissions beyond the permissions available to the assumed role.

### Important Difference

| Feature | Purpose |
|---|---|
| IAM Permissions Policy | Defines permissions that can be granted |
| Permissions Boundary | Sets a maximum permissions boundary for a user or role |
| SCP | Sets organization-level permission guardrails |
| Session Policy | Restricts permissions for a particular session |

These controls help enforce access restrictions at different levels.

---

## 10. How to Identify Unnecessary Permissions

Sometimes developers initially do not know exactly which AWS permissions an application requires.

AWS provides tools and services that help review access.

### 10.1 AWS CloudTrail

CloudTrail records AWS account activity, including API events.

You can review activity to understand which AWS actions are being performed.

For example:

- Which S3 actions did an application perform?
- Which role was assumed?
- Which API calls were made?

CloudTrail is useful for auditing and understanding actual usage.

### 10.2 IAM Access Analyzer

IAM Access Analyzer helps identify and analyze access to AWS resources.

It also supports generating IAM policies based on access activity recorded in CloudTrail.

This can help create policies that are closer to the actual permissions an application uses.

However, observed activity is not a guarantee that every required permission has been exercised. Test the generated policy before relying on it in production.

### 10.3 IAM Policy Simulator

The IAM Policy Simulator helps test how IAM policies affect access to AWS resources.

You can use it to understand whether a particular action is allowed or denied under the policies being evaluated.

### 10.4 Last Accessed Information

IAM provides last-accessed information that can help identify services and actions that have not been used recently.

It can support permission reviews, but should not be treated as proof that an unused permission is unnecessary in every situation.

---

## 11. Least Privilege for MERN and Next.js Developers

This principle is especially important when deploying full-stack applications on AWS.

Imagine the following architecture:

```text
React / Next.js Frontend
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

Your backend needs to upload user images to S3.

### Recommended Approach

1. Create a dedicated IAM Role for the backend.
2. Configure its trust policy to trust EC2.
3. Create a custom permissions policy.
4. Allow only the required S3 actions.
5. Restrict access to the required bucket and object prefix.
6. Attach the role to the EC2 instance.
7. Use the AWS SDK default credential provider chain to obtain temporary credentials.

### Avoid These Mistakes

- Do not expose AWS Secret Access Keys in frontend JavaScript.
- Do not store long-term AWS credentials in a publicly accessible repository.
- Do not give the backend `AdministratorAccess`.
- Do not reuse one highly privileged role for every application.
- Do not give a deployment pipeline permissions unrelated to deployment.

### Separate Permissions for Different Tasks

A production application may use different roles for different responsibilities:

| Identity / Role | Required Access |
|---|---|
| Backend EC2 Role | Required S3 operations |
| Image Processing Lambda Role | Required image and storage operations |
| CI/CD Deployment Role | Required deployment operations |
| Developer IAM Identity | Required development and debugging access |

Each role should have its own appropriate permissions.

---

## 12. Common Least Privilege Mistakes

Avoid these common mistakes:

1. Giving every developer `AdministratorAccess`.
2. Using `Action: "*"` without a valid reason.
3. Using `Resource: "*"` when specific resources can be defined.
4. Giving applications permissions to delete resources they only need to read.
5. Sharing one IAM identity between multiple people.
6. Storing long-term credentials inside application code.
7. Giving CI/CD pipelines unnecessary permissions.
8. Forgetting to review old or unused permissions.
9. Creating broad policies because of one permission error without investigating the actual missing action.
10. Assuming a Permissions Boundary or SCP grants permissions by itself.

**Remember:** When an application receives an Access Denied error, investigate the missing permission instead of immediately granting full access.

---

## 13. Least Privilege Best Practices

Follow these practices when designing AWS access:

- Start with minimum permissions.
- Grant access only to required actions.
- Restrict permissions to specific resources.
- Use conditions when additional restrictions are useful.
- Prefer IAM Roles and temporary credentials for AWS workloads.
- Use separate roles for separate applications and environments.
- Avoid unnecessary administrator permissions.
- Use MFA for human identities with console access.
- Review policies and permissions regularly.
- Monitor AWS activity through CloudTrail.
- Use IAM Access Analyzer and policy testing tools.
- Remove permissions that are no longer required.
- Document why each important permission is needed.

---

## 14. Practical Exercises

Use these exercises to test your understanding.

### Exercise 1: S3 Read Access

Create a policy that allows an application to:

- List one specific S3 bucket.
- Read objects from that bucket.
- Access no unrelated buckets.

Remember that `s3:ListBucket` uses the bucket ARN, while `s3:GetObject` uses object ARNs.

### Exercise 2: S3 Upload Access

Create a policy that allows an application to:

- Upload files into the `uploads/` prefix.
- Avoid deleting files.
- Avoid managing bucket configurations.

### Exercise 3: Developer Permissions

Imagine a developer only needs to view EC2 instance information.

Identify the permissions that are necessary and the permissions that should not be granted.

### Exercise 4: Review an Overly Broad Policy

Analyze this policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

Answer these questions:

1. Why is this policy overly broad?
2. What risks does it create?
3. How would you restrict its actions?
4. How would you restrict its resources?

### Exercise 5: MERN Backend Role

Design an IAM Role for a Node.js backend hosted on EC2.

Requirements:

- Trust EC2.
- Allow uploads to a specific S3 bucket prefix.
- Do not allow unrelated AWS service access.
- Use temporary credentials.

---

## 15. Learning Progress

### AWS IAM Concepts

- [x] IAM Users
- [x] IAM Groups
- [x] IAM Roles
- [x] IAM Policies
- [x] Principle of Least Privilege
- [x] Full Access vs Limited Access

### Least Privilege Learning Checklist

- [x] Understand the Principle of Least Privilege.
- [x] Understand why it is important.
- [x] Understand Full Access vs Limited Access.
- [x] Understand Over-Permissioning.
- [x] Understand `Action` and `Resource` restrictions.
- [x] Understand the role of IAM Policies.
- [x] Understand the risks of AdministratorAccess.
- [x] Understand Permissions Boundaries.
- [x] Understand Service Control Policies.
- [x] Understand Session Policies.
- [x] Understand CloudTrail and IAM Access Analyzer.
- [ ] Create and test least-privilege IAM policies in AWS.
- [ ] Apply least privilege to an EC2 application role.
- [ ] Review and reduce unnecessary permissions.

**Note:** The concepts and examples have been documented for learning. Practical implementation and testing will be completed separately.

---

## 16. Official AWS Documentation

Use these official AWS resources for deeper learning:

1. [IAM Security Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
2. [IAM Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
3. [Testing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
4. [IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
5. [Last Accessed Information](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html)
6. [Permissions Boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)
7. [Service Control Policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)

---

## Conclusion

The Principle of Least Privilege is one of the most important foundations of AWS security.

It ensures that every user, application, and service receives only the permissions required to perform its responsibilities.

IAM Policies define permissions, while the Principle of Least Privilege guides how narrowly those permissions should be granted.

For MERN and Next.js developers, this principle is especially important when working with EC2, S3, Lambda, CI/CD pipelines, and other AWS services.

**Next Step:** Complete practical IAM exercises and review the permissions of your AWS identities before moving to the next AWS service.

---

**Learning Path:** AWS Cloud & DevOps for MERN + Next.js Developers

**Topic:** AWS Identity and Access Management (IAM)

**Document:** Principle of Least Privilege

**Status:** Theory Completed | Practical Implementation Pending