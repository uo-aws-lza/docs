---
title: Subnets
parent: Networking
nav_order: 2
---

# Subnets

Subnets reside inside a [VPC](./vpcs.md), logically separating your services.
Subnets exist in **Availability Zones** (AZ)s, such as `us-west-2a` or
`us-west-2b`. The idea is that one AZ might go down, but if your system is
spanned across multiple, your service will stay up. This means you'll need to
configure multiple subnets, usually two, for each service.

This typically means to make your service highly available, you'll need to run
two or more instances. (TODO: mention application load balancers across AZs).

## Creating a Subnet in the default VPC

In the AWS portal, search for **Subnet** and click the one that lists itself as
a VPC feature. You'll be presented with a list of your current subnets. Click
the **Create subnet** button at the top right.

In the **VPC ID** dropdown, select the default VPC (or whichever you'd prefer).

Fill out **Subnet name** which is useful to name after the app that you'll be
hosting in it. Select an **Availability zone** and remember not to use the same
AZ for your second subnet. Choose the **IPv4 VPC CIDR block** where you want
this subnet to live, then create a new **IPv4 subnet CIDR block**.

Sizing is highly dependent on the application needs but I recommend starting
with `/24`, as it keeps your numbers easy to deal with, and each VPC comes with
a _huge_ amount of space by default.

Click **Create subnet**.

### Example

To create matching subnets in multiple AZs, my subnets might look like

```yaml
- name: myapp-2a
  AZ: us-west-2a
  IPv4 VPC CIDR block: 10.0.0.0/16
  IPv4 subnet CIDR block: 10.0.1.0/24
- name: myapp-2b
  AZ: us-west-2b
  IPv4 VPC CIDR block: 10.0.0.0/16
  IPv4 subnet CIDR block: 10.0.2.0/24
```

## Terraform

You'll need to provide the VPC ID either in the code or via the `TF_VAR_vpc_id`
environment variable.

```hcl
variable "vpc_id" {}

data "aws_vpc" "selected" {
  id = var.vpc_id
}

resource "aws_subnet" "myapp2a" {
  vpc_id     = aws_vpc.selected.id
  cidr_block = "10.0.1.0/24"
  availability_zone = "us-west-2a"

  tags = {
    Name = "myapp-2a"
  }
}

resource "aws_subnet" "myapp2b" {
  vpc_id     = aws_vpc.selected.id
  cidr_block = "10.0.2.0/24"
  availability_zone = "us-west-2b"

  tags = {
    Name = "myapp-2b"
  }
}
```
