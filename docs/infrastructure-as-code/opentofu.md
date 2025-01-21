---
title: OpenTofu CLI
parent: Infrastructure-as-Code
nav_order: 2
---

# OpenTofu

Up until recently, **Terraform** has been the go-to tool for infrastructure as code management. Unfortunately, HashiCorp has not only slowed development in the recent years, but also changed the licensing to a [BSL license](https://en.wikipedia.org/wiki/Business_Source_License), which could be problematic in the future if they start to require paying for use.

There's now an open-source fork of Terraform called OpenTofu that has been steadily outpacing Terraform in development and support. [OpenTofu is supported by the Linux Foundation](https://www.linuxfoundation.org/press/announcing-opentofu), so it's expected to be well-supported and remain open-source for years to come.

For this reason, we'll be installing and using OpenTofu instead of Terraform.

## Installation

You can find multi-platform install instructions here: [https://opentofu.org/docs/intro/install/](https://opentofu.org/docs/intro/install/)

## Optional: Useful Alias

To make my life easier, I prefer to alias `tofu` to `tf`.

### Mac/Linux

In your `./bashrc` or other shell configuration file:

```bash
alias tf='tofu'
```

### Windows

In your Powershell `$PROFILE`:

```powershell
Set-Alias -Name tf -Value tofu
```
