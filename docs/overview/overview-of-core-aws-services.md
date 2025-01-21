---
title: Overview of Core AWS Services
parent: Overview
nav_order: 3
---

# Overview of Core AWS Services

AWS offers a wide variety of services that power everything from simple web applications to complex, large-scale data processing. In this guide, we’ll introduce you to some of the most commonly used “core” AWS services that you’ll likely work with at the University of Oregon. Since your account grants you **AdministratorAccess**, you’ll be able to explore these services in depth.

---

## Overview

- **Goal**: Gain familiarity with the AWS services you’re most likely to use, including their basic concepts and typical use cases.
- **Prerequisites**:
  - A UO-provisioned AWS account (with AdministratorAccess).
  - Basic understanding of cloud terminology (e.g., instances, buckets).

---

## Amazon EC2 (Elastic Compute Cloud)

**Purpose**: Launch virtual servers (“instances”) in the cloud.

1. **What It Is**:  
   - EC2 lets you run applications on virtual machines. You choose the instance size, operating system, and networking options.
   
2. **Common Use Cases**:  
   - Hosting websites, running development and production environments, data processing.

3. **Key Concepts**:  
   - **AMI (Amazon Machine Image)**: A template for your instance’s OS and pre-installed software.  
   - **Instance Types**: Determine CPU, memory, storage, and networking capacity.  
   - **Security Groups**: Act like a virtual firewall for your instances.

> **Next Steps**: Learn how to launch your first EC2 instance in the TODO(link) [Creating an EC2 Instance](#) guide.

---

## Amazon S3 (Simple Storage Service)

**Purpose**: Store objects and files (e.g., images, documents, backups) in a highly scalable, durable, and secure way.

1. **What It Is**:  
   - S3 is an object storage service where you place files (“objects”) into buckets.

2. **Common Use Cases**:  
   - Data backup, static website hosting, archiving, media storage and delivery.

3. **Key Concepts**:  
   - **Buckets**: Containers for your data (must have a globally unique name).  
   - **Objects**: The files you store in a bucket.  
   - **Bucket Policies & Access Control**: Manage who can read/write objects.  
   - **Storage Classes**: Different pricing and performance tiers (Standard, Infrequent Access, Glacier, etc.).

> **Next Steps**: Explore creating and managing S3 buckets, versioning, and lifecycle rules.

---

## Amazon RDS (Relational Database Service)

**Purpose**: Easily set up, operate, and scale relational databases in the cloud.

1. **What It Is**:  
   - RDS automates common database tasks such as provisioning, patching, and backups.

2. **Supported Engines**:  
   - MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, and Amazon Aurora.

3. **Key Concepts**:  
   - **DB Instances**: Managed database servers you can launch.  
   - **Multi-AZ Deployment**: High availability setup across different availability zones.  
   - **Read Replicas**: Scale out read-heavy workloads.

> **Next Steps**: Read about the best practices for RDS sizing, backups, and security group configurations.

---

## AWS Lambda

**Purpose**: Run your code without provisioning or managing servers (Serverless Computing).

1. **What It Is**:  
   - Lambda functions are short pieces of code triggered by events (e.g., file uploads to S3, API calls, or scheduled jobs).

2. **Common Use Cases**:  
   - Event-driven processing (e.g., thumbnail generation), serverless APIs, scheduled data tasks, and automation.

3. **Key Concepts**:  
   - **Triggers**: Events that invoke your Lambda function.  
   - **Execution Role**: IAM role granting your function permission to interact with AWS resources.  
   - **Function Configuration**: Memory, timeout, environment variables.

> **Next Steps**: Try creating a simple Lambda function and invoke it manually or through an event trigger.

---

## AWS Identity and Access Management (IAM)

Although we covered IAM policies in detail, here’s a quick overview:

1. **What It Is**:  
   - IAM is the security backbone, controlling user and service access within AWS.

2. **Key Concepts**:  
   - **Users / Roles**: Entities that can perform actions (e.g., you, applications).  
   - **Policies**: JSON documents defining allowed or denied actions.  
   - **SSO**: Single Sign-On mechanism UO uses for centralized access management.

> **Next Steps**: Review our TODO(link) [Understanding Basic IAM Policies](#) guide if you haven’t already.

---

## Additional Helpful Services

### Amazon VPC (Virtual Private Cloud)
- **Purpose**: Define and manage your own virtual network in AWS, controlling IP ranges, subnets, and routing.

### Terraform
- **Purpose**: Manage resources as code for repeatable deployments.  
- **Tip**: Terraform/OpenTofu is popular at UO for infrastructure automation. Check out our TODO(link) [Infrastructure as Code (Terraform/OpenTofu)](#) guide.

### Amazon CloudWatch
- **Purpose**: Monitor performance metrics, set alarms, and collect logs from your AWS resources.

### AWS CloudTrail
- **Purpose**: Track API calls across your AWS infrastructure for auditing and compliance.

### Amazon Route 53
- **Purpose**: Manage DNS for your domains, enabling custom domain names and routing policies.

---

## Best Practices for Exploring Services

1. **Start Small**: Experiment with small-scale test environments to learn the basics.  
2. **Use Tags**: Tag your resources (e.g., `Project:Test`) to keep them organized.  
3. **Monitor Costs**: Even if you have admin rights, it’s crucial to track and manage cloud spend.  
4. **Security First**: Always follow the principle of least privilege for IAM roles and keep your resources protected (e.g., via security groups, encryption).

---

## Next Steps

- **Ready to Launch**: Head to TODO(link) [Creating an EC2 Instance](#) for a hands-on tutorial.
- **Keep Learning**: Check out additional guides on S3 best practices, RDS configurations, and more.

---

## Troubleshooting & Support

- Need help deciding which service to use? Submit a support request to the UO Cloud Team at TODO(link) [Cloud Services Support](https://service.uoregon.edu/cloud-support).  

