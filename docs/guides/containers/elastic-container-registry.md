---
title: Elastic Container Registry
parent: Containers
---

# Overview

Let's set up an Elastic Container Registry (ECR) to host our private docker
images for use with other services such as ECS.

## Prerequisites

Make sure you've configured your CLI using our
[aws-cli guide](../../getting-started/aws-cli.md). You should be able to run the
following command, replacing with your profile name, and it should give you some
long hashed output:

`aws ecr get-login-password --region us-west-2 --profile PROFILE`

## Create an ECR

### AWS Website

In the search bar, search for **Elastic Container Registry**, and open it. At
the top right, click **Create repository**.

The only field this guide cares about is the _Repository name_, which should be
comprised of two names:

- **Namespace**: You can use this to group similar repositories. Something like
  the name of your application should suffice.

- **Repo-Name**: This is the actual name of the repository. Use whatever you
  like.

For this example, I'll use `myapp/myapp`.

Once you click **Create**, you'll have an ECR.

For the rest of the guide, you'll need the ECR URI, which you can find and copy
right from the list of repositories.

### Terraform

The following block of Terraform code will create the same ECR as above:

```hcl
resource "aws_ecr_repository" "ecr" {
    name = "myapp/myapp"
}

output "ecr_uri" {
    value = aws_ecr_repository.ecr.repository_url
}
```

This will output your ECR URI, which you'll need for the rest of this guide.

## Pushing an Image to your ECR

Now that you have a configured ECR and your ECR URI, let's push an image using
Docker.

Set an environment variable to hold this URI for ease of typing commands:

```bash
export ECR_URI="YOUR_URI_HERE"
```

Let's assume you already have your container built and tagged as `myapp:v1.0.0`.

> [!NOTE] If you don't know how to build a container, see our
> [introduction to containers guide](./introduction-to-containers.md)

You need to tag your container with the repository url

```bash
docker tag myapp:v1.0.0 "${ECR_URI}:v1.0.0"`
```

Now log into your ECR with the following command

```bash
aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_URI
```

Finally, you can simply push the image

```bash
docker push "${ECR_URI}:v1.0.0"
```
