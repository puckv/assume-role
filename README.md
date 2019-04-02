# assume-role

Shell script to assume AWS role, using either default credentials or GPG encrypted account credentials, with support for MFA code. 

## Usage

```bash
assume-role [accountId] roleName
```

Account ID can be omitted if you are assuming role within the same account as your current identity. 

This can be executed either by an IAM user, assumed role, EC2 instance, etc. You should make sure that AWS credentials are available to AWS CLI through one of standard methods.

If your current identity is an IAM user, her MFA device ID will be retrieved and you will be asked for a MFA code. The user should have IAM permission _iam:ListMFADevices_. _assume-role_ expects user to have a MFA configured.

Assumed role temporary credentials will be displayed as output, ready to be pasted to your terminal:

```
export AWS_ACCESS_KEY_ID=xxxxx
export AWS_SECRET_ACCESS_KEY=xxxxx
export AWS_SESSION_TOKEN=xxxxx
```

You can automatically set these as your shell variables by executing _assume-role_ this way:

```
eval $(assume-role roleName)
```

#### Using GPG-encrypted credentials

If no AWS credentials are available when _assume-role_ is executed, it will look for `~/.aws/credentials.gpg` file and try to decrypt it. It should contain `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` separated by a space.
