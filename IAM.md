### INTRODUCTION TO AWS IDENTITY AND ACCESS MANAGEMENT
In AWS Identity and Access Management (IAM), managing the security and access rights of users and groups is essential for protecting your AWS resources.

An **IAM user** is an entity within your AWS account that represents a person or service that interacts with AWS. Users can interact with AWS services directly or through APIs, depending on the permissions assigned to them.

#### Types of Access for Users
1. **Programmatic Access**: This access type enables an IAM user to interact with AWS services through the API, CLI, or SDKs. AWS provides these users with an access key ID and a secret access key, which are used to authenticate requests programmatically.

2. **AWS Management Console Access**: This allows the user to log in to the AWS Management Console with a username and password. This type of access is suitable for users who need to interact with AWS services through the graphical interface.

#### Permissions
Permissions define what actions users are allowed to perform on AWS resources. Permissions for IAM users can be managed in the following ways:
- **Inline Policies**: These are policies that you create and manage directly attached to a single IAM user. They are directly embedded into the user and are not shared with other users.
  
- **Managed Policies**: Managed policies are standalone policies that can be attached to multiple users, groups, or roles. AWS provides managed policies (which are created and managed by AWS) or you can create your own custom managed policies.

### IAM Groups

An **IAM group** is a collection of IAM users. Groups help you specify permissions for multiple users, which makes it easier to manage the permissions for those users collectively rather than individually.

#### Group-Based Permission Management
- When a user is added to a group, they inherit all the permissions associated with that group. This allows you to apply a single set of permissions to multiple users, simplifying the administration of IAM permissions.
- Permissions in groups are managed through policies. You can attach both AWS managed policies and custom managed policies to groups.
Sure, let's define the core components of AWS IAM—users, groups, policies, and roles—before diving into the detailed steps of the scenario.


#### **IAM Policies**
An **IAM policy** is a JSON document that formally specifies one or more permissions. Policies define what actions are allowed or denied on which AWS resources. There are two types of policies:
- **Managed policies**: These are standalone policies that you can attach to multiple users, groups, or roles within AWS.
- **Inline policies**: These are policies that you create and manage and are embedded directly into a single user, group, or role.

#### **IAM Roles**
An **IAM role** is a set of permissions that don’t apply directly to a user but instead are intended to be assumed by someone who needs them temporarily. Roles are used in various scenarios, such as granting permissions to applications running on EC2 instances or allowing users from one AWS account to access resources in another.

### Creating IAM Group, User, and Modifying Policies


#### Step 1: Create a Group and Attach a Read-Only Policy
1. **Log into the AWS Management Console** and navigate to the IAM dashboard.
2. **Create a new IAM group**:
   - Click on **Groups** in the sidebar, then click **Create New Group**.
   - Name the group `DevOps`.
   - Continue to the next step to attach policies.
3. **Attach a Read-Only Policy**:
   - Search for `ReadOnlyAccess` in the list of policies.
   - Select the `ReadOnlyAccess` policy, which grants read-only access to most AWS services.
   - Click **Next Step** and then **Create Group**.

#### Step 2: Create an IAM User and Add to the Group
1. **Create an IAM user**:
   - Navigate to **Users** and click **Add user**.
   - Enter a user name, e.g., `JohnDoe`.
   - Select both **Programmatic access** and **AWS Management Console access**.
   - Set a custom password or auto-generate one, and decide whether the user must reset the password upon first login.
   - Click **Next: Permissions**.
2. **Add the user to the DevOps group**:
   - Select **Add user to group**.
   - Check the box next to the `DevOps` group.
   - Click **Next: Tags** (optional: add any relevant tags), then **Next: Review**, and **Create user**.

#### Step 3: User Login and Access Test
1. **User logs in**:
   - JohnDoe logs into the AWS Management Console using their credentials.
2. **Test access**:
   - Attempt to view resources across AWS services. The user should only be able to view, not modify any settings or data.
   - Try to access EC2 instances, expecting failure due to the ReadOnlyAccess policy.

#### Step 4: Modify Policy to Grant EC2 Access
1. **Adjust group policies**:
   - Return to the IAM dashboard, select the `DevOps` group, and click on **Attach policy**.
   - Search for and select `AmazonEC2FullAccess`.
   - Detach the `ReadOnlyAccess` policy if full access to EC2 is now intended, or keep both if read access to other services is still desired.
   - Click **Attach policy**.

#### Step 5: Retest Access
1. **JohnDoe retests access**:
   - After the policy update, log in again and attempt to launch or manage EC2 instances.
   - Access should now be granted based on the new policy settings.
