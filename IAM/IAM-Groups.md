# AWS IAM Groups (Identity and Access Management)

## 1. Introduction to IAM Groups

AWS Identity and Access Management (IAM) Groups are collections of IAM Users that help administrators manage permissions for multiple users together.

Instead of attaching the same permissions individually to every IAM User, administrators can create an IAM Group and attach policies to that group.

All IAM Users who belong to that group automatically receive the permissions assigned to the group.

### Real-World Example

Suppose a software company has five developers:

- Developer 1
- Developer 2
- Developer 3
- Developer 4
- Developer 5

All five developers need access to read files from an Amazon S3 bucket.

Without IAM Groups, an administrator would need to attach the same S3 permissions individually to every developer.

With IAM Groups:

1. Create an IAM Group named `Developers`.
2. Attach the required S3 policy to the group.
3. Add all five IAM Users to the group.

Now all five users receive the permissions assigned to the Developers group.

This makes permission management easier, more organized, and consistent.

---

## 2. Understanding IAM Groups

An IAM Group is an identity that contains multiple IAM Users.

Its main purpose is to simplify permission management by allowing administrators to assign permissions to multiple users at once.

Important characteristics:

- An IAM Group can contain multiple IAM Users.
- A single IAM User can belong to multiple IAM Groups.
- Policies can be attached to IAM Groups.
- All members receive permissions granted through their groups.
- IAM Groups cannot contain other IAM Groups.
- IAM Groups cannot contain IAM Roles.
- IAM Groups cannot be used to sign in to AWS.
- An IAM Group does not have its own login credentials.
- IAM Groups are associated with a specific AWS account.

### Important Concept

IAM Groups are not separate AWS accounts.

They are simply a way to organize IAM Users and manage their permissions collectively.

Every IAM User still has its own identity and credentials.

---

## 3. Why Do We Need IAM Groups?

Managing permissions individually becomes difficult when an organization has many users.

Consider an organization with:

- 10 Frontend Developers.
- 15 Backend Developers.
- 5 DevOps Engineers.
- 3 Administrators.

Each team requires different AWS permissions.

If permissions are managed individually, administrators must repeatedly attach, update, and remove policies for multiple users.

IAM Groups solve this problem.

### Without IAM Groups

An administrator must:

1. Open an individual IAM User.
2. Attach the required policy.
3. Repeat the process for every user.
4. Update every user separately when permissions change.

### With IAM Groups

An administrator can:

1. Create a group for a particular team.
2. Attach the required policies to that group.
3. Add users to the group.
4. Manage permissions centrally.

When the group's policies change, the updated permissions apply to its members.

---

## 4. IAM Group Components

IAM Groups contain several important components and relationships.

### A. Group Name

A group name identifies an IAM Group.

Examples:

```text
Developers
FrontendTeam
BackendTeam
DevOpsTeam
Administrators
```

Group names must be unique within the AWS account.

AWS IAM group names support letters, numbers, and certain special characters.

### B. Group Members

Group members are IAM Users added to the group.

Example:

```text
Group: BackendTeam

Members:
- backend-user-1
- backend-user-2
- backend-user-3
```

All members receive the permissions attached to the group.

### C. Attached Policies

Policies define the permissions assigned to an IAM Group.

Example:

```text
Group: FrontendTeam

Attached Policy:
AmazonS3ReadOnlyAccess
```

Every IAM User in this group receives the permissions granted by the attached policy.

### D. Group ARN

Every IAM Group has an Amazon Resource Name (ARN).

An IAM Group ARN generally follows this structure:

```text
arn:aws:iam::ACCOUNT_ID:group/GROUP_NAME
```

Example:

```text
arn:aws:iam::123456789012:group/BackendTeam
```

ARNs uniquely identify AWS resources and are used in IAM policies and other AWS operations.

---

## 5. IAM Groups and IAM Policies

IAM Groups work together with IAM Policies.

An IAM Policy defines what actions are allowed or denied.

An IAM Group allows administrators to apply those permissions to multiple IAM Users.

### Example

Suppose an organization has three backend developers.

All three need permission to upload files to a particular S3 bucket.

An administrator creates a policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-products-bucket/*"
    }
  ]
}
```

The administrator then attaches this policy to the `BackendTeam` group.

All users who belong to this group receive the S3 upload permission.

### Permission Flow

```text
                 IAM GROUP
                BackendTeam
                     |
                     |
              Attached Policy
                     |
              S3 Upload Access
                     |
          -----------------------
          |          |          |
        User 1     User 2     User 3
          |          |          |
       Upload     Upload     Upload
        Files      Files      Files
```

This is the main purpose of IAM Groups: applying the same permissions to multiple IAM Users.

---

## 6. IAM Group Membership Rules

AWS IAM Groups have some important membership rules.

### Rule 1: Multiple Users in One Group

An IAM Group can contain multiple IAM Users.

Example:

```text
Developers
  |
  |-- User A
  |-- User B
  |-- User C
  |-- User D
```

All four users receive the permissions attached to the Developers group.

### Rule 2: One User Can Belong to Multiple Groups

An IAM User can be a member of multiple IAM Groups.

Example:

```text
User: Israr

Groups:
- Developers
- S3-Access
- TestingTeam
```

The user receives applicable permissions from all these groups.

### Rule 3: Groups Cannot Be Nested

An IAM Group cannot contain another IAM Group.

For example:

```text
Developers
    |
    |-- BackendTeam
    |
    |-- FrontendTeam
```

This structure is not supported in AWS IAM Groups.

Groups can contain IAM Users only.

### Rule 4: No Default Group

AWS does not automatically create a group that contains every IAM User.

If administrators need a common group for all users, they must create it and add users manually.

---

## 7. Types of IAM Group Permissions

IAM Groups can receive permissions through IAM Policies.

### A. AWS Managed Policies

These are predefined policies created and maintained by AWS.

Examples:

- AmazonS3ReadOnlyAccess
- AmazonS3FullAccess
- AmazonEC2ReadOnlyAccess
- AdministratorAccess

Example:

An administrator can attach `AmazonS3ReadOnlyAccess` to a group named `FrontendTeam`.

All members receive the permissions granted by that policy.

### B. Customer Managed Policies

These are custom policies created and maintained by AWS account administrators.

Example:

An organization creates a custom policy that allows developers to upload files only to a specific S3 bucket.

The policy can then be attached to the required IAM Group.

### C. Inline Policies

Inline policies can also be attached directly to IAM Groups.

These policies are associated with a specific group and are not independent managed policies.

They can be useful for permissions that are intended only for one particular group.

---

## 8. IAM Group vs IAM User vs IAM Role

These three IAM concepts have different purposes.

| Feature | IAM User | IAM Group | IAM Role |
|---------|----------|-----------|-----------|
| Purpose | Represents an identity | Organizes IAM Users | Provides an assumable identity |
| Credentials | Can have credentials | No credentials | Temporary credentials when assumed |
| Login | Traditional console login supported | Cannot sign in | No standard IAM Role password |
| Permissions | Can receive policies | Policies apply to members | Policies define role permissions |
| Contains Users | No | Yes | No |
| Can Be Assumed | Not in the same way as a role | No | Yes |
| Example | Backend Developer | BackendTeam | EC2 accessing S3 |

### Simple Explanation

- IAM User = Individual identity.
- IAM Group = Collection of IAM Users.
- IAM Role = Identity that can be assumed by an authorized entity.

---

## 9. How Permissions Work with Multiple IAM Groups

An IAM User can belong to multiple groups.

In this situation, the user receives applicable permissions from the policies attached to those groups, along with any other applicable policies.

### Example

Suppose a user belongs to two groups:

```text
User: BackendDeveloper

Group 1: Developers
Policy: AmazonEC2ReadOnlyAccess

Group 2: S3-Access
Policy: AmazonS3ReadOnlyAccess
```

The user receives:

- EC2 Read-Only Access.
- S3 Read-Only Access.

This allows administrators to organize permissions based on responsibilities.

### Important: Explicit Deny

AWS evaluates applicable policies when determining whether a request is allowed.

If an applicable policy explicitly denies an action, that Deny overrides an Allow.

For example:

- Group A allows S3 access.
- Group B explicitly denies access to a particular S3 resource.

The explicit Deny takes precedence.

---

## 10. Creating an IAM Group (Traditional IAM Workflow)

The traditional AWS IAM Console supports creating IAM Groups.

General steps:

### Step 1: Open AWS Console

Sign in to the AWS Management Console using an authorized identity.

### Step 2: Search for IAM

Search for IAM in the AWS Console search bar.

### Step 3: Open User Groups

In the traditional IAM interface, navigate to:

```text
IAM → User groups
```

### Step 4: Create Group

Select:

```text
Create group
```

### Step 5: Enter Group Name

Example:

```text
BackendTeam
```

### Step 6: Add Users

Select the IAM Users that should belong to this group.

### Step 7: Attach Policies

Select the required policies.

For example:

```text
AmazonS3ReadOnlyAccess
```

Or attach an appropriate custom policy.

### Step 8: Create Group

Review the group name, selected users, and policies.

Select Create group.

The IAM Group is now created.

### Important

The exact interface and available options may differ depending on the AWS Console experience and the access available to your account.

---

## 11. Adding Users to an Existing IAM Group

Administrators can add existing IAM Users to an IAM Group.

General steps:

1. Open the IAM Console.
2. Navigate to User groups.
3. Select the required group.
4. Open the Users tab.
5. Select Add users.
6. Choose the IAM Users.
7. Confirm Add users.

The selected users become members of the group and receive its applicable permissions.

### AWS CLI Command

The following AWS CLI command adds an existing IAM User to an IAM Group:

```bash
aws iam add-user-to-group \
  --user-name backend-user \
  --group-name BackendTeam
```

---

## 12. Removing Users from an IAM Group

An administrator can remove an IAM User from a group when the user no longer requires that group's permissions.

General steps:

1. Open the IAM Console.
2. Navigate to User groups.
3. Select the required group.
4. Open the Users tab.
5. Select the user.
6. Choose Remove users.
7. Confirm the operation.

### AWS CLI Command

```bash
aws iam remove-user-from-group \
  --user-name backend-user \
  --group-name BackendTeam
```

### Important

Removing a user from a group removes the permissions inherited through that group.

However, the user may still have permissions through:

- Other IAM Groups.
- Policies attached directly to the user.
- Other applicable AWS authorization mechanisms.

Removing a user from one group does not automatically remove every permission from that user.

---

## 13. Managing IAM Groups

Administrators can perform different management operations on IAM Groups.

Common operations include:

### Create Group

Create a new IAM Group.

### View Group

View group members and attached policies.

### Add Users

Add existing IAM Users to the group.

### Remove Users

Remove IAM Users from the group.

### Attach Policy

Attach a managed policy to the group.

### Detach Policy

Remove an attached managed policy.

### Edit Inline Policy

Modify the inline permissions associated with the group.

### Delete Group

Delete a group when it is no longer required.

Important:

Before deleting an IAM Group, review its members and permissions.

You must remove its users and detach or delete its policies before deleting the group.

---

## 14. IAM Groups and the New AWS Experience

AWS has introduced a newer account and team management experience.

In this newer experience, human users can be invited and managed through the Team page.

Some traditional IAM management options may not appear in the newer interface.

Important distinctions:

- Traditional IAM Groups are still supported in AWS IAM.
- Existing IAM Groups can continue to be used in supported IAM environments.
- Existing IAM Users can continue receiving permissions through IAM Groups.
- The traditional IAM documentation still covers group creation, membership, and policy management.
- IAM Identity Center has its own separate users and groups.
- IAM Groups and IAM Identity Center groups are not the same thing.

### Why Does AWS Recommend IAM Identity Center?

For workforce access, AWS recommends using federation and centralized identity management.

IAM Identity Center helps organizations manage human users and groups and provide access to multiple AWS accounts and applications.

For modern workforce access, organizations generally use IAM Identity Center instead of creating long-term IAM Users for every employee.

However, understanding traditional IAM Groups remains valuable when managing existing AWS infrastructure and legacy environments.

---

## 15. IAM Groups Security Best Practices

IAM Groups help organize permissions, but they must be configured carefully.

Important security recommendations:

### 1. Organize Groups by Responsibilities

Create groups according to actual job responsibilities.

Examples:

- FrontendTeam
- BackendTeam
- DevOpsTeam
- DatabaseAdministrators

### 2. Follow Least Privilege

Attach only the policies required by the group's members.

Avoid unnecessary AdministratorAccess.

### 3. Avoid Unnecessary Group Membership

Do not add users to groups unless they require the permissions provided by those groups.

### 4. Review Group Policies

Regularly review attached policies and remove unnecessary permissions.

### 5. Review Group Members

Remove users who no longer require access.

### 6. Avoid Excessive Permissions

Do not attach broad policies when a smaller custom policy can provide the required access.

### 7. Use MFA for Human Users

Require MFA through the appropriate identity-management configuration.

### 8. Audit Group Membership

Regularly check which users belong to which groups.

This helps identify unnecessary access and security risks.

---

## 16. IAM Groups in Real-World Development

Consider a software company that manages multiple AWS services.

The company has different development teams.

Each team requires different permissions.

### Example: Software Development Organization

| IAM Group | Required Permissions |
|-----------|----------------------|
| FrontendTeam | Required frontend and S3 read access |
| BackendTeam | Required backend and S3 upload access |
| DevOpsTeam | Required infrastructure management |
| DatabaseTeam | Required database management |
| TestingTeam | Required testing and monitoring access |

### Example Group Structure

```text
AWS Account
    |
    |-- FrontendTeam
    |      |-- frontend-user-1
    |      |-- frontend-user-2
    |
    |-- BackendTeam
    |      |-- backend-user-1
    |      |-- backend-user-2
    |
    |-- DevOpsTeam
    |      |-- devops-user-1
    |
    |-- TestingTeam
           |-- tester-user-1
           |-- tester-user-2
```

Each group receives permissions according to its responsibilities.

When a new developer joins the company, the administrator can add that developer to the appropriate group instead of manually attaching every required policy.

This makes AWS access management easier as the organization grows.

---

## 17. Important IAM Group Concepts Learned

This section summarizes the concepts covered in this lesson.

| Concept | What I Learned |
|---------|----------------|
| IAM Group | A collection of IAM Users |
| Group Name | Identifies an IAM Group |
| Group Members | IAM Users belonging to a group |
| Attached Policies | Define permissions for group members |
| Group ARN | Uniquely identifies an IAM Group |
| Multiple Users | A group can contain multiple users |
| Multiple Groups | A user can belong to multiple groups |
| Group Nesting | IAM Groups cannot contain other groups |
| Default Group | AWS does not create a default group |
| AWS Managed Policy | A predefined policy maintained by AWS |
| Customer Managed Policy | A custom policy created by an administrator |
| Inline Policy | A policy attached directly to a group |
| Group Membership | Controls which users receive group permissions |
| Explicit Deny | Overrides an applicable Allow |
| IAM Identity Center | Modern centralized workforce identity management |

---

## 18. Learning Progress

### Completed Topics

- [x] Introduction to IAM Groups.
- [x] Understanding IAM Groups.
- [x] Why IAM Groups are needed.
- [x] IAM Group Components.
- [x] Group Name.
- [x] Group Members.
- [x] Attached Policies.
- [x] Group ARN.
- [x] IAM Groups and IAM Policies.
- [x] Group Membership Rules.
- [x] Multiple Users in One Group.
- [x] One User in Multiple Groups.
- [x] Groups Cannot Be Nested.
- [x] Types of Group Policies.
- [x] IAM User vs IAM Group vs IAM Role.
- [x] Permissions from Multiple Groups.
- [x] Explicit Deny.
- [x] Traditional IAM Group Creation Workflow.
- [x] Adding Users to Groups.
- [x] Removing Users from Groups.
- [x] Managing IAM Groups.
- [x] AWS CLI Group Commands.
- [x] New AWS Experience Differences.
- [x] IAM Groups Security Best Practices.
- [x] Real-World Development Examples.

### Current Learning Status

- Topic: AWS Identity and Access Management (IAM)
- Subtopic: IAM Groups
- Status: Completed - Theoretical Learning
- Practical Implementation: Not completed yet.

---

## 19. Official AWS Documentation

The following official AWS resources are useful for further learning and reference:

1. IAM User Groups:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html

2. Creating IAM Groups:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_create.html

3. Adding and Removing Users from Groups:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_add-remove-users.html

4. Viewing IAM Groups:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_list.html

5. Changing IAM User Permissions:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html

6. IAM Policies and Access Management:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/access_controlling.html

7. IAM Security Best Practices:

   https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html

8. IAM Identity Center:

   https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html

---

## Conclusion

IAM Groups are an important part of traditional AWS Identity and Access Management.

They allow administrators to organize IAM Users and manage permissions for multiple users through a centralized approach.

Instead of assigning the same policies individually to every user, administrators can attach policies to groups and add users to the appropriate groups.

Understanding IAM Groups is valuable when working with existing AWS infrastructure, managing legacy environments, and understanding how traditional AWS permission management works.

Although AWS recommends IAM Identity Center for modern workforce identity management, traditional IAM Groups remain supported and are still important to understand.

### Learning Goal

Understand how IAM Groups are created, organized, managed, and used to distribute permissions to multiple IAM Users.