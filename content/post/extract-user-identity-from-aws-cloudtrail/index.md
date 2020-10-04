---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Extract User Identity from AWS Cloudtrail"
subtitle: ""
summary: ""
profile: false
authors:
  - david-xiao
tags:
  - cybersecurity
  - aws
  - cloudtrail
  - iam
  - user-identity
categories:
  - Information Security
  - AWS
  - IAM
date: 2020-09-15
lastmod: 2020-09-15
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## Why Do I Care

Cloudtrail is an essential service in AWS that provides the source of truth on what has happened at API and event level.

Whether you are troubleshooting or investigating something on AWS, being able to look up user identity across the Cloudtrail event logs can be very helpful.

By default AWS provides 90 days of event history and you can look up on key fields such as `User name`, `event time` or `event id`.

In some cases that's all you need.

But there are cases where you need to go beyond the 90 days and want to be able to extract user identity information from Cloudtrail logs directly.

For example, you may wish to write a Lambda function to auto-tag any new EC2 instances with username of the creator, eventid, eventtime when a user is creating new EC2 instances.

For another example, you may need to search history go past 90 days to look for information like `WHO did WHAT and WHEN`.

In those cases, understand the JSON structure of Cloudtrail log and specifically the identity related portion comes handy.

## eventType

Cloudtrail records various types of events. In each JSON record, `eventType` indicates the type of the event. Each event type has a different JSON structure.

The following types cover the most cases I'm aware of but the list is not intended to be exhaustivee - I will add to it as I learn.

### AwsApiCall

API call is the most common event type. It represents an API call on an AWS service.

The great thing about this event type is it can be triggered on CloudWatch event. *update: recently CloudWatch Event is renamed to AWS EventBridge*.

### AwsConsoleSignIn

This type of event is generated when a user signed in on AWS management console.

### AwsServiceEvent

Services such as AWS SSO generates such type of event when authenticating or federating a user.

## userIdentity.type

On each record, the userIdentity block represents the identity information. Various types of userIdentity exists. The most common ones are: `IAMUser`, `AssumedRole`, `AWSService`, `SAMLUser` and `Unknown`.

### IAMUser

The below json is extracted from a Cloudtrail event that represents an API call made by an IAM user. User name can be extracted from the `userIdentity.userName` field.

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAUWQOET4WMTL6OV3SZ",
    "arn": "arn:aws:iam::323225952045:user/tool-poc",
    "accountId": "323225952045",
    "accessKeyId": "AKIAUWQOET4WFCRTJDF5",
    "userName": "tool-poc"
  },
  "eventID": "1e85a381-9e58-4612-a8d5-abc30ff95f65",
  "eventType": "AwsApiCall",
  ...
}
```

In another example, the eventType is different but userIdentity block structure looks similar.

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAVBHXPSQ567GPQHO75",
    "arn": "arn:aws:iam::346263884858:user/admin",
    "accountId": "346263884858",
    "userName": "admin"
  },
  "eventTime": "2020-09-12T18:05:04Z",
  "eventSource": "signin.amazonaws.com",
  "eventName": "ConsoleLogin",
  "awsRegion": "us-east-1",
  "eventID": "0b8f0958-8507-4526-b8f5-d56741ccae77",
  "eventType": "AwsConsoleSignIn",
  ...
}
```

### AssumedRole

AssumedRole is when an identity assumes an AWS role. The identity could be IAM user in the same account, user from another AWS account, AWS service or a SAML provider.

Below are a few examples.

User name: alice@example.com

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJKUFA6XAMROQBJRNA:alice@example.com",
    "arn": "arn:aws:sts::323225952045:assumed-role/assume-admin-role-an-account/alice@example.com",
    "accountId": "323225952045",
    ...
  },
  "eventID": "e7f3be2f-a81b-4a87-975f-eaac58faca9e",
  "eventType": "AwsApiCall",
  ...
}
```

User name: AutoScaling

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJ6TYGYS2TFMOQYEY2:AutoScaling",
    "arn": "arn:aws:sts::323225952045:assumed-role/AWSServiceRoleForAutoScaling/AutoScaling",
    "accountId": "323225952045",
    ...
  },
  "eventID": "b67837c3-f90c-49c9-8750-02adef205f64",
  "eventType": "AwsApiCall",
  ...
}
```

User name: bob@example.com

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAI4O72XO7XFD2BHDUA:bob@example.com",
    "arn": "arn:aws:sts::323225952045:assumed-role/Sandbox-SSO-PowerUser/bob@example.com",
    "accountId": "323225952045",
    ...
  },
  "eventID": "5921eee9-7a54-4672-84d5-9a64a81822e4",
  "eventType": "AwsApiCall",
  ...
}
```

User name: test@example.com

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAVBHXPSQ577YYUL4QC:test@example.com",
    "arn": "arn:aws:sts::346263884858:assumed-role/AWSReservedSSO_AWSAdministratorAccess_33ca3b9a1184d671/test@example.com",
    "accountId": "346263884858",
    ...
  },
  "eventID": "a1b2f460-0288-4937-b850-12b521a10230",
  "eventType": "AwsApiCall",
  ...
}
```

User name: AssumeRoleSession

```yaml
{
  "eventVersion": "1.07",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAIUHYOXFSUYZJIJQUM:AssumeRoleSession",
    "arn": "arn:aws:sts::323225952045:assumed-role/CloudHealth/AssumeRoleSession",
    "accountId": "323225952045",
    "accessKeyId": "ASIAUWQOET4WKMRLT5G6",
    "sessionContext": {
      "sessionIssuer": {
        "type": "Role",
        "principalId": "AROAIUHYOXFSUYZJIJQUM",
        "arn": "arn:aws:iam::323225952045:role/CloudHealth",
        "accountId": "323225952045",
        "userName": "CloudHealth"
      },
      "attributes": {
        "creationDate": "2020-09-15T13:53:25Z",
        "mfaAuthenticated": "false"
      }
    }
  },
  "eventTime": "2020-09-15T14:08:27Z",
  "eventSource": "dynamodb.amazonaws.com",
  "eventName": "ListTables",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "34.230.249.2",
  "eventID": "de0f486d-c1ff-4032-9e86-17ba166f687e",
  "eventType": "AwsApiCall",
  ...
}
```

### SAMLUser

This type of userIdentity are most commonly seen in `AssumeRoleWithSAML` event.

User name: bob@example.com

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "SAMLUser",
    "principalId": "6DLJuKNu+27u3kwvB9BKCv71kco=:bob@example.com",
    "userName": "bob@example.com",
    "identityProvider": "6DLJuKNu+27u3kwvB9BKCv71kco="
  },
  "eventTime": "2020-09-08T13:22:03Z",
  "eventSource": "sts.amazonaws.com",
  "eventName": "AssumeRoleWithSAML",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "72.21.217.22",
  "eventID": "892c55be-2ab1-4e0e-a80f-5e04f05b625d",
  "eventType": "AwsApiCall",
  ...
}
```

### AWSService

For this type of userIdentity, it simply does not have a real user. Instead, it's AWS service that is performing an action.

User name     (blank)

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AWSService",
    "invokedBy": "elasticbeanstalk.amazonaws.com"
  },
  "eventTime": "2020-09-15T13:43:16Z",
  "eventSource": "sts.amazonaws.com",
  "eventName": "AssumeRole",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "elasticbeanstalk.amazonaws.com",
  "eventID": "93fd006e-a58f-4304-a9a1-04136ca8a1c3",
  "eventType": "AwsApiCall",
  ...
}
```

### Unknown

This is commonly seen in `AwsServiceEvent` event. I've seen AWS SSO produces this type of event but I'm not aware of what else AWS services produce it.

User name: test@example.com

```yaml
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "Unknown",
    "principalId": "90677f325d-ffd9565d-ac85-4753-8dc6-502c67f1c727",
    "accountId": "346263884858",
    "userName": "test@example.com"
  },
  "eventTime": "2020-09-15T13:35:04Z",
  "eventSource": "sso.amazonaws.com",
  "eventName": "Authenticate",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "75.15.150.17",
  ...
  "eventID": "63e0001b-e2fa-49b0-bf29-b7c92d977266",
  "eventType": "AwsServiceEvent",
  "recipientAccountId": "346263884858"
}
```
