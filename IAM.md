# Introduction to AWS Identity and Access Management (IAM)

## Overview

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS services and resources. 
It allows you to manage **users**, **groups**, and **roles** and set up **permissions** to allow or deny access to AWS resources. 
IAM ensures secure and granular access control across your AWS environment.

## Key Concepts

1. **Users**: 
   - Represents individual users in AWS.
   - Can be assigned permissions either directly or through group membership.
   - Each user can have security credentials (like access keys and passwords) that allow them to interact with AWS resources.

2. **Groups**: 
   - A collection of IAM users. You can use groups to specify permissions for multiple users, making it easier to manage permissions for similar users.
   - Permissions assigned to a group apply to all users in the group.

3. **Roles**: 
   - An IAM role is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. 
   - Roles do not have credentials (passwords or access keys); instead, AWS services or external users assume roles to gain temporary access.
   
4. **Policies**: 
   - Policies are JSON documents that define permissions for an identity (users, groups, or roles).
   - Managed Policies: Created and managed by AWS or customers. Can be reused across multiple identities.
   - Inline Policies: Embedded directly in a single user, group, or role.

5. **Permission Boundaries**:
   - Permission boundaries control the maximum permissions an IAM entity (user or role) can have. Even if policies attached to the entity allow broader permissions,
   - the boundary restricts what actions the entity can perform.

6. **AWS Organizations and SCPs**: 
   - AWS Organizations helps manage multiple AWS accounts. Using Service Control Policies (SCPs), you can set permission guardrails for accounts within your organization.

## IAM Features

- **Granular Permissions**: Control access to AWS resources at the service level (e.g., allow S3 access but not EC2 access).
- **Secure Resource Access**: Define who can access which services and resources, ensuring security.
- **Multi-Factor Authentication (MFA)**: Add an extra layer of protection by requiring a second factor (such as a code from a mobile app) when logging in.
- **Temporary Security Credentials**: Use IAM roles to grant temporary access to users or services without the need to share credentials.
- **Access Analyzer**: Identify and review resources that are shared with external entities, improving security by showing unintended access.

## Common IAM Use Cases

1. **Creating and Managing Users**: 
   - Use IAM to create users with unique security credentials.
   - Assign each user the minimum required permissions following the principle of least privilege.

2. **Assuming Roles**: 
   - Use roles to grant temporary access to resources, especially for cross-account access or granting permissions to AWS services like EC2 or Lambda.

3. **Managing Access Keys and Passwords**: 
   - Users can have AWS Management Console access (with username and password) and/or programmatic access (with access keys).

4. **Federating Users with External Identity Providers**: 
   - Use IAM to federate users with external identity providers such as Active Directory, SAML-based systems, or social identity providers.

## Best Practices for AWS IAM

1. **Enable MFA**: Protect AWS accounts by enabling Multi-Factor Authentication (MFA) for users.
2. **Use Roles for Applications**: Applications should use IAM roles for access rather than embedding access keys in code.
3. **Grant Least Privilege**: Grant users, groups, and roles the minimum permissions they need to perform their tasks.
4. **Rotate Security Credentials Regularly**: Regularly rotate access keys and passwords to minimize risk.
5. **Monitor Access and Usage**: Use AWS CloudTrail to log IAM activity for auditing purposes. Review logs to track changes and access patterns.
6. **Use Groups to Manage Permissions**: Instead of attaching policies to individual users, assign them to groups for better scalability and manageability.

## Creating IAM Users and Groups

### Creating an IAM User

1. **Sign in to the AWS Management Console** and open the **IAM console** at https://console.aws.amazon.com/iam/.
2. In the **navigation pane**, choose **Users**, and then select **Add user**.
3. Enter a **username** and select the access type:
   - **Programmatic access** for API, CLI, SDK, and other AWS development tools.
   - **AWS Management Console access** for web-based console access.
4. Attach policies to the user directly or add the user to a group.
5. Complete the process to create the user and provide credentials to the user.

### Creating an IAM Group

1. Open the **IAM console**.
2. In the **navigation pane**, choose **Groups**, and select **Create New Group**.
3. Name the group and select policies to attach to the group.
4. Add users to the group.

## Creating IAM Roles

1. In the **IAM console**, navigate to **Roles** and select **Create role**.
2. Choose the type of role based on its use case (e.g., another AWS service like EC2 or an external identity provider).
3. Attach the required permission policies to the role.
4. Optionally, set up trust relationships to define who can assume the role.

## IAM Policy Structure

IAM policies are defined in JSON. Here’s an example of a policy that grants full access to Amazon S3:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
```

### Policy Elements

- **Version**: Specifies the policy language version (always use "2012-10-17").
- **Effect**: Either `Allow` or `Deny`.
- **Action**: Specifies the actions that the policy allows or denies (e.g., `s3:*` for all actions on S3).
- **Resource**: Specifies the AWS resources the policy applies to.
