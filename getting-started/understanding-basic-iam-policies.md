# Understanding Basic IAM Policies

In AWS, **Identity and Access Management (IAM)** is the service that lets you control access to AWS resources. Here at the University of Oregon, you’ll typically log into AWS via **Single Sign-On (SSO)**. After authentication, you’ll assume an IAM role that grants you specific permissions. While you may not need to create or manage policies directly, having a basic understanding of how IAM policies work will help you better navigate AWS resources and request the right access when needed.

---

## Overview

- **Goal**: Understand what IAM policies are, how they relate to roles and permissions, and how they’re managed in a UO AWS Organization environment.
- **Prerequisites**:
  - A University of Oregon AWS account (accessed via SSO).
  - Basic familiarity with the TODO(link) [AWS Console](#).

---

## Step 1: What Are IAM Policies?

An **IAM policy** is a JSON document that defines which AWS actions are allowed or denied. Think of it as a set of rules that outline what a user (or role) can and cannot do in AWS.

**Key points about IAM policies:**

1. **Identity-based Policies**: Attached to an IAM user, group, or role. These policies let the identity perform certain actions on certain resources.
2. **Resource-based Policies**: Attached directly to a resource (e.g., S3 bucket policy). These policies specify who can access that resource.
3. **Service Control Policies (SCPs)**: Applied at the AWS Organization or Organizational Unit (OU) level. SCPs can limit what services or actions can be used across all accounts in an organization.

At UO, your access is managed primarily through **Identity-based Policies** assigned to your role. Organization-wide restrictions (SCPs) may also limit what services or regions you can use for compliance or budget reasons.

---

## Step 2: Roles vs. Users vs. Groups

- **Users**: In many AWS environments, individual users have unique credentials. At UO, however, you generally won’t use a dedicated AWS user login. Instead, you’ll use **SSO**.
- **Roles**: Roles are like “permission sets” that can be assumed. You’ll typically see roles such as `AdminRole`, `PowerUserRole`, or `ReadOnlyRole`. Once you log in via SSO, you pick the appropriate role to assume.
- **Groups**: A way to manage multiple users at once. In SSO-based setups, large group-level management is often handled outside AWS by identity providers (e.g., Okta, Active Directory). If you see groups in IAM, they typically map to roles or permission sets for the entire organization.

---

## Step 3: Viewing IAM Policies in the Console

You can inspect the policies attached to your role in the AWS Console:

1. **Navigate to IAM**:
   - From the AWS Console, choose **Services** > **IAM**.

2. **Check Roles**:
   - Select **Roles** from the left navigation panel.
   - Search for or select the role you assumed when logging in (e.g., `AdminRole`, `PowerUserRole`).
   - Click on the role to see the **Permissions** tab.

3. **Review Attached Policies**:
   - Under the **Permissions** tab, you’ll see **Policies** attached to your role.
   - You can click each policy name to see its JSON, which outlines the allowed or denied actions.

> **Tip**: If you don’t see your role here, it’s possible that your SSO role references a permission set managed in AWS Single Sign-On (IAM Identity Center). You may also see a link or reference to the permission set in IAM.

---

## Step 4: Common Policy Types You May Encounter

1. **AdministratorAccess**  
   - Grants full access to nearly all AWS services and resources.
   - Typically reserved for advanced users or cloud administrators.

2. **PowerUserAccess**  
   - Allows you to manage AWS services but restricts some account-level actions (e.g., IAM user management).

3. **ReadOnlyAccess**  
   - Lets you view resources but not modify them.

4. **Custom Policies**  
   - Created by the UO Cloud Team to align with specific departmental or project needs (e.g., limiting certain services, restricting creation of certain resource types).

---

## Step 5: Requesting Policy Changes or New Permissions

If you find you don’t have enough access to perform your tasks (for example, you can’t launch a certain type of resource):

1. **Identify the Needed Actions**  
   - Look up the specific service actions you require (e.g., `ec2:RunInstances`) or the resource you need to modify.

2. **Contact the UO Cloud Team**  
   - Create a support request and provide details at TODO(link) [Cloud Support Request](https://service.uoregon.edu/cloud-support). 
   - Clarify the use case and why you need the access.

3. **Await Policy Update**  
   - If approved, the Cloud Team will either update your existing role or create a custom policy/role that grants the requested permissions.

---

## Best Practices

- **Use Least Privilege**: Only request the permissions you actually need. This keeps the environment more secure and reduces the chance of accidental changes.
- **Avoid Sharing Roles**: If you need to collaborate, each person should assume their own role. This ensures proper tracking of who did what in AWS.

---

## Next Steps

- **Move on to the TODO(link) [Overview of Core AWS Services](#)** to learn about popular AWS services you’ll likely use (EC2, S3, RDS, etc.).
- If you’re ready for hands-on tasks, try TODO(link) [Creating an EC2 Instance](#) to practice applying your new IAM knowledge.

---

_You now have a foundational understanding of how IAM policies work in a UO-managed AWS Organization. Continue exploring other guides or reach out to the Cloud Team if you need further assistance._

