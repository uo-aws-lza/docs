---
title: Application Load Balancer
parent: Networking
nav_order: 2
---

# Application Load Balancer

_The Application Load Balancer distributes incoming HTTP and HTTPS traffic
across multiple targets such as Amazon EC2 instances, microservices, and
containers, based on request attributes. When the load balancer receives a
connection request, it evaluates the listener rules in priority order to
determine which rule to apply, and if applicable, it selects a target from the
target group for the rule action._

You'll want to configure an Application Load Balancer (ALB) to sit in front of
your EC2 or ECS instances to provide a safe, redundant, single point of ingress
for your services.

## Creating an Application Load Balancer

### Creating your Target Group

In the AWS portal, search for **Target groups** and select the one listed as a
**EC2 feature**. Click **Create target group** at the top right.

If you're creating this for EC2 instances, you can use **Instances** as the
target type, otherwise, choose **IP addresses**.

Give your target group a name and leave all the port and protocol information
default.

Select the VPC where your application will live in the **VPC** dropdown and
click **Next**.

On this page, remove the listed IPv4 addresses so there's none, then at the
bottom, click the **Create** button.

### Creating the ALB

In the AWS portal, search for **Load balancers**, and select the one listed as
an **EC2 feature**. Click the **Create load balancer** button at the top right,
then select **Application Load Balancer** from the options.

Give your loadbalancer a name, probably something related to the application
you're deploying. Leave it as **Internet-facing** and **IPv4**.

Select the VPC you want it in, typicall the same VPC where your application
subnets will live. See our [VPC guide](./vpcs.md) or
[Subnets guide](./subnets.md) for more information.

For the **Availability Zones and subnets**, select your two subnets you created
for your application to be zone-redundant (see guides).

Select your application's **Security group** from the dropdown. See our
[Security Groups guide](./security-groups.md) for more information.

For **Listeners and routing**, you'll need to create a new **target group**,
which you can do by clicking the little link titled **Create target group**.
Select this target group for the **Default action** box. You can select either
**HTTP** or **HTTPS** for the **Protocol** field, usually **HTTPS**.

If you've selected **HTTPS**, for the TLS options, there are several ways to get
a certificate. If you don't want to `uoregon.edu` domain, you can use **ACM** to
get an Amazon certificate. Otherwise, you can go through the UO's service portal
to request a new TLS certificate that you can use with the **Import
certificate** button.

Scroll all the way down and click the **Create load balancer** button.

## Terraform

You'll need to gather some resource IDs first:

- VPC ID: `TF_VAR_vpc_id`
- Subnet1: `TF_VAR_subnet_a_id`
- Subnet2: `TF_VAR_subnet_b_id`
- Security Group: `TF_VAR_security_group_id`

These can be set by various means, see our
[variables guide](../infrastructure-as-code/variables.md) for more information
on passing variables to Terraform/OpenTofu.

```hcl
variable "vpc_id" {}
variable "subnet_a_id" {}
variable "subnet_b_id" {}
variable "security_group_id" {}

data "aws_vpc" "selected" {
  id = var.vpc_id
}
data "aws_subnet" "subneta" {
  id = var.subnet_a_id
}
data "aws_subnet" "subnetb" {
  id = var.subnet_b_id
}
data "aws_security_group" "sg" {
  id = var.security_group_id
}

resource "aws_lb" "alb" {
  name               = "myapp-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [data.aws_security_group.sg.id]
  subnets            = [
    data.aws_subnet.subneta.id,
    data.aws_subnet.subnetb.id
  ]

  enable_deletion_protection = true
}

resource "aws_lb_target_group" "tg" {
  name        = "myapp-tg"
  port        = 80
  protocol    = "HTTP"
  target_type = "ip"
  vpc_id      = var.vpc_id

  health_check {
    healthy_threshold   = 2
    unhealthy_threshold = 2
    timeout             = 5
    interval            = 30
    path                = "/"
    matcher             = "200"
  }

  tags = {
    Name = "myapp-tg"
  }
}

# HTTP listener that redirects all traffic to HTTPS
resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.alb.arn
  port              = "80"
  protocol          = "HTTP"

  default_action {
    type = "redirect"

    redirect {
      protocol    = "HTTPS"
      port        = "443"
      status_code = "HTTP_301"
    }
  }
}

# HTTPS listener with SSL certificate and forwarding to the target group
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.alb.arn
  port              = "443"
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-2016-08"
  certificate_arn   = var.certificate_arn

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg.arn
  }
}
```

TODO(test all this)
