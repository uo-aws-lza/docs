---
title: AWS CLI
parent: Tools
nav_order: 1
---

# AWS CLI

Performing any actions in AWS with either the LZA or Terraform requires an authenticated session to AWS using the AWS CLI.

The official installation instructions are here: [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), follow the instructions for your platform to ensure that the AWS CLI is installed, and you can run the aws --version  command in your terminal:

```
➜  ~ aws --version
aws-cli/2.22.7 Python/3.12.7 Darwin/23.6.0 source/arm64
```

## Creating the AWS SSO Configuration

The AWS CLI has two concepts for authentication, **session** and **profile**. You can think of the **session** as the AWS Organization that you're connecting to, in our case, `uoregon`, and the **profile** is like the individual AWS Account.

{: .note}
You should have received a few pieces of information upon the delivery of your AWS Account. We'll be referencing `ACCOUNT-ID` and `START-URL` in the following setup guide

To configure the initial `uoregon` session and a `ACCOUNT-ID_ASSUMED-ROLE` profile for working with the management AWS account, `ACCOUNT-ID_AWSAdministratorAccess`:

```bash
aws configure sso --profile ACCOUNT-ID_AWSAdministratorAccess
```

_Note: To keep things simple, always use `ACCOUNT_ID\_ASSUMED-ROLE` as the profile name when creating a profile. This keeps it straightforward when sharing terraform repositories._

When you first set up the CLI, it will ask you about session information:

- **SSO session Name:** `uoregon` 
- **SSO start URL:** `START-URL`
- **SSO region:** `us-west-2`  
- **SSO registration scopes:** Press enter to accept the default

It will then open a browser and have you authenticate with your DuckID. Upon success, you can close the browser tab and return to the CLI.

_Now that the session is configured, we'll use this session for all future profiles we create._

The CLI will then ask you a few more questions:

- Select the account to associate it with the `ACCOUNT-ID_AWSAdministratorAccess` profile you're creating
- For the default client region, use `us-west-2` 
- Press enter to accept the default output format of None

Your CLI is now configured to use the `aws` command to manage resources! Here's an example output, yours should look similar:

```
➜  ~ aws configure sso
SSO session name (Recommended): uoregon
SSO start URL [None]: <START-URL>
SSO region [None]: us-west-2
SSO registration scopes [sso:account:access]:
Attempting to automatically open the SSO authorization page in your default browser.
If the browser does not open or you wish to use a different device to authorize this request, open the following URL:

https://oidc.us-west-2.amazonaws.com/authorize?response_type=code&client_id=BIGLONGAUTHORIZESTRING
There are 3 AWS accounts available to you.
Using the account ID <ACCOUNT-ID>
The only role available to you is: AWSAdministratorAccess
Using the role name "AWSAdministratorAccess"
CLI default client Region [None]: us-west-2
CLI default output format [None]:
```

When you run `aws configure sso --profile <PROFILE_NAME>` in the future, the same SSO session can be selected, many accounts can run from the same SSO.

## Authenticating via SSO

To authenticate against AWS for your CLI, which you will need to periodically do whenever you receive 403 errors, use the following command:

```bash
aws sso login --sso-session <SESSION_NAME>
```

`<SESSION_NAME>` for the above example would be `uoregon`, and this will refresh your credentials for all accounts using that SSO login site.

When provided a profile name (`<ACCOUNT_ID>\_<ASSUMED_ROLE>`) tools like terraform, and the AWS CLI will use the appropriate SSO session to grab current credentials for the profile.

```terraform
provider "aws" {
  region  = "us-west-2"
  profile = "ACCOUNT-ID_AWSAdministratorAccess"
}
```

{: .warning}
Credentials in `~/.aws/credentials` will be prioritized over SSO cached profiles. If you're running into 403s despite refreshing your SSO session, make sure there isn't a conflicting profile in `~/.aws/credentials`.
