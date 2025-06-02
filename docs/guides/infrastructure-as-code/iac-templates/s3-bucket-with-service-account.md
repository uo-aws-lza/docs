---
title: 'S3 Bucket with Read/Write Service Account'
parent: IaC Templates
---

# S3 Bucket with Read/Write Service Account

The following Terraform/OpenTofu code will create an IAM service account user, IAM user access key, S3 bucket, and bucket policy enabling read/write access for the service account user.

```hcl
// create an IAM service account user
resource "aws_iam_user" "s3-service-account" {
  name = "s3-service-account"
}

// create an IAM access key for the service account
resource "aws_iam_access_key" "s3-service-account" {
  user = aws_iam_user.s3-service-account.name
}

// create a Secrets Manager secret to store the access key
resource "aws_secretsmanager_secret" "s3-service-account-credentials" {
  name                    = "s3-service-account-credentials"
  recovery_window_in_days = 7
}

// store the access key in the secret
resource "aws_secretsmanager_secret_version" "s3-service-account-credentials" {
  secret_id = aws_secretsmanager_secret.s3-service-account-credentials.id
  secret_string = jsonencode({
    access_key_id     = aws_iam_access_key.s3-service-account.id
    secret_access_key = aws_iam_access_key.s3-service-account.secret
  })
}

// create the S3 bucket that the service account will access
resource "aws_s3_bucket" "archive" {
  bucket_prefix = "archive-"
}

// write a policy document that will allow the service account to use the S3 bucket
data "aws_iam_policy_document" "allow_access_from_s3_service_account" {
  statement {
    principals {
      type = "AWS"
      identifiers = [
        aws_iam_user.s3-service-account.arn,
      ]
    }

    actions = [
      "s3:ListBucket",
      "s3:GetBucketLocation",
      "s3:ListBucketMultipartUploads",
      "s3:ListBucketVersions",
    ]

    effect = "Allow"

    resources = [
      aws_s3_bucket.archive.arn,
    ]
  }

  statement {
    principals {
      type = "AWS"
      identifiers = [
        aws_iam_user.s3-service-account.arn,
      ]
    }

    actions = [
      "s3:GetObject",
      "s3:PutObject",
      "s3:DeleteObject",
      "s3:AbortMultipartUpload",
      "s3:ListMultipartUploadParts",
    ]

    effect = "Allow"

    resources = [
      "${aws_s3_bucket.archive.arn}/*",
    ]
  }
}

// use the policy document as the bucket policy for the new bucket
resource "aws_s3_bucket_policy" "allow_access_from_s3_service_account" {
  bucket = aws_s3_bucket.archive.id
  policy = data.aws_iam_policy_document.allow_access_from_s3_service_account.json
}
```

Once the above code is applied you can acess the IAM service account credentials by navigating to the created secret in Secrets Manager and clicking "Retrieve secret value".
