---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Audit User Actions Using CloudTrail - Part 2"
subtitle: "Cross-Account Access via AssumeRole"
summary: "This is the second post of a series that demonstrates how to leverage AWS CloudTrail in auditing user actions. This post is focused on cross account access."
authors:
  - david-xiao
tags: []
categories: []
date: 2020-09-17T23:53:30-04:00
lastmod: 2020-09-17T23:53:30-04:00
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

## Overview

When auditing system events or performing an investigation to understand what happened, it is imperative to identify the IAM principal, to establish traceability and timelines.

In context of AWS CloudTrail, it means looking up events pertaining to the IAM principal and actions in question as well as looking for useful information inside such events.

When a user assumes role cross-account in a multi-account environment, it can be done two ways: either programatically or via AWS management console.

Since either way generates different CloudTrail events, I will disuss two examples in this post respectively.

## Assume Role Programmatically Cross-Account

## Assume Role via AWS Console Cross-Account

A typical investigation flow that involves cross-account assumerole goes like this:

- Step 1: Identify an event on CloudTrail that needs investigation

- Step 2: Identify the closest `AssumeRole` event that happens before the event in question
- Step 3: Locate the closest `SwitchRole` event that happens at the same time of `AssumeRole`. If found, it indicates the user session was established via AWS Console
- Step 4: If you have access to the Identity Account, locate the AssumeRole

In the following example, we investigate a "suspicious" `CreateUser` event.

Identity Account: `203016562928`

IAM username: `bob@example.com`

Role Account: `776613361644`

Role Name: `assume-admin-role-example`

CreateUser

```json
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJBP4A5WSXVNY72RLE:bob@example.com",
    "arn": "arn:aws:sts::776613361644:assumed-role/assume-admin-role-example/bob@example.com",
    "accountId": "776613361644",
    "accessKeyId": "ASIA3JUODO7W6YEVI655",
    "sessionContext": {
      "sessionIssuer": {
        "type": "Role",
        "principalId": "AROAJBP4A5WSXVNY72RLE",
        "arn": "arn:aws:iam::776613361644:role/assume-admin-role-example",
        "accountId": "776613361644",
        "userName": "assume-admin-role-example"
      },
      "webIdFederationData": {},
      "attributes": {
        "mfaAuthenticated": "true",
        "creationDate": "2020-09-17T19:04:10Z"
      }
    }
  },
  "eventTime": "2020-09-17T19:05:08Z",
  "eventSource": "iam.amazonaws.com",
  "eventName": "CreateUser",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "75.15.154.15",
  "userAgent": "console.amazonaws.com",
  "requestParameters": {
    "userName": "test-no-permission",
    "tags": []
  },
  "responseElements": {
    "user": {
      "path": "/",
      "userName": "test-no-permission",
      "userId": "AIDA3JUODO7W7VCWFDJMM",
      "arn": "arn:aws:iam::776613361644:user/test-no-permission",
      "createDate": "Sep 17, 2020 7:05:08 PM"
    }
  },
  "requestID": "cc58c060-fe96-4678-b0bf-b888f12bf008",
  "eventID": "38d0221b-61e0-47d6-9c45-7eb2dc55125b",
  "eventType": "AwsApiCall",
  "recipientAccountId": "776613361644"
}
```


AssumeRole
```json
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AWSAccount",
    "principalId": "AIDAS6RF25DXSM2CA5KAD",
    "accountId": "203016562928"
  },
  "eventTime": "2020-09-17T19:04:10Z",
  "eventSource": "sts.amazonaws.com",
  "eventName": "AssumeRole",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "AWS Internal",
  "userAgent": "AWS Internal",
  "requestParameters": {
    "roleArn": "arn:aws:iam::776613361644:role/assume-admin-role-example",
    "roleSessionName": "bob@example.com"
  },
  "responseElements": {
    "credentials": {
      "accessKeyId": "ASIA3JUODO7WTIO2PI64",
      "expiration": "Sep 17, 2020 8:04:10 PM",
      "sessionToken": "IQoJb3JpZ2luX2VjEAMaCXVzLWVhc3QtMSJIMEYCIQDsEwOWhE/9cNh+Xpg+V6r8ug3ULRnoOCPNCQhorh13xgIhAIfc+u8ttYNsjLQJRMvo7EnDXMkOAViMFuU7Mma8zjGEKqMCCBwQARoMNzc2NjEzMzYxNjQ1Igyt06eMsyzHkQJRK84qgAIF/jUW99caWE07piwWI2EGTHU2KMx6ioRz3uDbDS24GKy3XvQRalC+5YTZoOQDQvpziRmO33BEzM6Ws5TBTggo/yXGAJRQthB8IqiGkbsClbOG8cYsuhRXK3+yK8OHhCSfr0ehO2SYNiaqEClyT9n8QtEmkQawN56IiOoE9HBzTA7xxYbj7XULL/okog7D3l18NG32rxhHS1ACDN3ro9RGjrPicn9PHFfBqvK+uP3JJQVlcQZu6yGVvtH3rIiYdwAo5bSEc3G9G/LSEiwh47o7NrTOzrRsznARBgefSPs9K3qIgMpZHs3DgqJ0TID1k5y1w4KlPvL36C/+LEmxckNYMKrmjvsFOpwBGDQAVghEOsvntE328Yt2M9Yv0x55cZ5RPJ2pGtQW4geb8+aT2ThZ1zSGwlnvMM8TE+HAwUs0+GQwZbp5UpIiDYLzUeZ0pYWVBHmv/YzN3w0bSKTrC8Jc/0aAaUnxmKMkH5AWO6pBelw8KtVIvd9BwKgBQKVo+tsAVGEdKbVTlwOvLcuhWonCvjoxPCiPgNR05HF8QANbpWet0p+k"
    },
    "assumedRoleUser": {
      "assumedRoleId": "AROAJBP4A5WSXVNY72RLE:bob@example.com",
      "arn": "arn:aws:sts::776613361644:assumed-role/assume-admin-role-example/bob@example.com"
    }
  },
  "requestID": "fdbb008c-63ce-4207-8171-b041d6f38672",
  "eventID": "40b4d219-0448-436f-9420-cdd3dc654b44",
  "resources": [
    {
      "accountId": "776613361644",
      "type": "AWS::IAM::Role",
      "ARN": "arn:aws:iam::776613361644:role/assume-admin-role-example"
    }
  ],
  "eventType": "AwsApiCall",
  "recipientAccountId": "776613361644",
  "sharedEventID": "19ee34b2-52bd-4dfa-8c8e-cf68344062a6"
}
```

SwitchRole
```json
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJBP4A5WSXVNY72RLE:bob@example.com",
    "arn": "arn:aws:sts::776613361644:assumed-role/assume-admin-role-example/bob@example.com",
    "accountId": "776613361644"
  },
  "eventTime": "2020-09-17T19:04:10Z",
  "eventSource": "signin.amazonaws.com",
  "eventName": "SwitchRole",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "75.15.154.15",
  "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.102 Safari/537.36",
  "requestParameters": null,
  "responseElements": {
    "SwitchRole": "Success"
  },
  "additionalEventData": {
    "SwitchFrom": "arn:aws:iam::203016562928:user/bob@example.com",
    "RedirectTo": "https://console.aws.amazon.com/console/home"
  },
  "eventID": "70627092-0c9c-4163-9975-42ffcc50a37a",
  "eventType": "AwsConsoleSignIn",
  "recipientAccountId": "776613361644"
}
```


in the source account look up for AssumeRole around the same time with the same "sharedEventID"
AssumeRole

```json
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAS6RF25DXSM2CA5KAD",
    "arn": "arn:aws:iam::203016562928:user/bob@example.com",
    "accountId": "203016562928",
    "accessKeyId": "ASIAS6RF25DXQR3PH2AX",
    "userName": "bob@example.com",
    "sessionContext": {
      "sessionIssuer": {},
      "webIdFederationData": {},
      "attributes": {
        "mfaAuthenticated": "true",
        "creationDate": "2020-09-17T13:58:45Z"
      }
    },
    "invokedBy": "AWS Internal"
  },
  "eventTime": "2020-09-17T19:04:10Z",
  "eventSource": "sts.amazonaws.com",
  "eventName": "AssumeRole",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "AWS Internal",
  "userAgent": "AWS Internal",
  "requestParameters": {
    "roleArn": "arn:aws:iam::776613361644:role/assume-admin-role-example",
    "roleSessionName": "bob@example.com"
  },
  "responseElements": {
    "credentials": {
      "accessKeyId": "ASIA3JUODO7WTIO2PI64",
      "expiration": "Sep 17, 2020 8:04:10 PM",
      "sessionToken": "IQoJb3JpZ2luX2VjEAMaCXVzLWVhc3QtMSJIMEYCIQDsEwOWhE/9cNh+Xpg+V6r8ug3ULRnoOCPNCQhorh13xgIhAIfc+u8ttYNsjLQJRMvo7EnDXMkOAViMFuU7Mma8zjGEKqMCCBwQARoMNzc2NjEzMzYxNjQ1Igyt06eMsyzHkQJRK84qgAIF/jUW99caWE07piwWI2EGTHU2KMx6ioRz3uDbDS24GKy3XvQRalC+5YTZoOQDQvpziRmO33BEzM6Ws5TBTggo/yXGAJRQthB8IqiGkbsClbOG8cYsuhRXK3+yK8OHhCSfr0ehO2SYNiaqEClyT9n8QtEmkQawN56IiOoE9HBzTA7xxYbj7XULL/okog7D3l18NG32rxhHS1ACDN3ro9RGjrPicn9PHFfBqvK+uP3JJQVlcQZu6yGVvtH3rIiYdwAo5bSEc3G9G/LSEiwh47o7NrTOzrRsznARBgefSPs9K3qIgMpZHs3DgqJ0TID1k5y1w4KlPvL36C/+LEmxckNYMKrmjvsFOpwBGDQAVghEOsvntE328Yt2M9Yv0x55cZ5RPJ2pGtQW4geb8+aT2ThZ1zSGwlnvMM8TE+HAwUs0+GQwZbp5UpIiDYLzUeZ0pYWVBHmv/YzN3w0bSKTrC8Jc/0aAaUnxmKMkH5AWO6pBelw8KtVIvd9BwKgBQKVo+tsAVGEdKbVTlwOvLcuhWonCvjoxPCiPgNR05HF8QANbpWet0p+k"
    },
    "assumedRoleUser": {
      "assumedRoleId": "AROAJBP4A5WSXVNY72RLE:bob@example.com",
      "arn": "arn:aws:sts::776613361644:assumed-role/assume-admin-role-example/bob@example.com"
    }
  },
  "requestID": "fdbb008c-63ce-4207-8171-b041d6f38672",
  "eventID": "14fb06e3-5649-4fc3-a274-226ba85c8be6",
  "resources": [
    {
      "accountId": "776613361644",
      "type": "AWS::IAM::Role",
      "ARN": "arn:aws:iam::776613361644:role/assume-admin-role-example"
    }
  ],
  "eventType": "AwsApiCall",
  "recipientAccountId": "203016562928",
  "sharedEventID": "19ee34b2-52bd-4dfa-8c8e-cf68344062a6"
}
```

## Reference

[How to Audit Cross-Account Roles Using AWS CloudTrail and Amazon CloudWatch Events](https://aws.amazon.com/blogs/security/how-to-audit-cross-account-roles-using-aws-cloudtrail-and-amazon-cloudwatch-events/)
