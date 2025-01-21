---
title: Managing Your Budget
parent: Getting Started
nav_order: 2
---

# Managing Your Budget

Nobody likes an unexpected cloud bill. 
AWS enables you to create configurable budgets and/or alarms that will notify you or take specific actions when your expected spend is... unexpected.

## Billing Alarms

A billing alarm is a simple notification that will alert you when your expected spend is over some threshold.

[Check out their official guide to creating a billing alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)

## AWS Budgets

Budgets are more complicated than simple billing alarms, but can be configured to perform certain actions in response to unexpected usage, such as turning off EC2 instances.

First, you'll need to [create a budget using a simplified template](https://docs.aws.amazon.com/cost-management/latest/userguide/budget-templates.html).

If you want to configure response actions, you'll want to [configure a budget action](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-action-configure.html).
