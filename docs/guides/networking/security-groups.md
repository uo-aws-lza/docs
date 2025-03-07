---
title: Security Groups
parent: Networking
nav_order: 2
---

# Security Groups

You can think of a Security Group (SG) as a set of firewall rules that apply to
any resource they're associated with. The following security group is **not**
best practice, but gives you an examle of how they work.

## Creating a Security Group

Let's create a simple SG to secure communications to our service that should
**only** be accessible via the following ports and locations

- `8080`: Primary application port. Allow from ANY
- `22`: Administrative management. Allow from my 2FA VPN space,
  `123.111.90.0/24`

In the AWS portal, search for **Security Group**, and click the one listed as
**VPC feature**.

You'll be presented with a list of your current SGs. Click the **Create security
group** button at the top right.

Give it a **Security group name** and **Description**, probably something
related to the application you're deploying. Select a **VPC** where you want
this SG. This VPC should also be where your application subnets live. See our
[Subnet guide](./subnets.md) for more information.

For **Inbound rules**, I'll click **Add rule**, type `8080` in the **Port
range** field. I'll click **Add rule** again, select **SSH** from the **Type**
field, then type `123.111.90.0/24` in the **Source** field.

For **Outbound rules**, I'll click **Add rule**, select **All traffic** for my **Type** field.

Click **Create security group**


## Terraform

You'll need to provide the VPC ID either in the code or via the `TF_VAR_vpc_id`
environment variable.

```hcl
variable "vpc_id" {}

data "aws_vpc" "selected" {
  id = var.vpc_id
}

resource "aws_security_group" "myapp" {
  name        = "myapp"
  description = "Allow traffic for myapp"
  vpc_id      = aws_vpc.selected.id

  tags = {
    Name = "myapp"
  }
}

# Inbound rules
resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv4" {
  security_group_id = aws_security_group.myapp.id
  cidr_ipv4         = aws_vpc.selected.cidr_block
  from_port         = 8080
  ip_protocol       = "tcp"
  to_port           = 8080
}

resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv6" {
  security_group_id = aws_security_group.myapp.id
  cidr_ipv6         = aws_vpc.selected.ipv6_cidr_block
  from_port         = 8080
  ip_protocol       = "tcp"
  to_port           = 8080
}

# Outbound rules
resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv4" {
  security_group_id = aws_security_group.myapp.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv6" {
  security_group_id = aws_security_group.myapp.id
  cidr_ipv6         = "::/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}
```
