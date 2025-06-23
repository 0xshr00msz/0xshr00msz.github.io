---
title: TryHackMe — Hackfinity Cloud Challenge 1 Writeup
date: 2025-06-23 20:14
categories: [Cloud, CTF]
tags: [TryHackMe, AWS, Offensive]
author: 0xshr00msz
image:
  path: /assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/hackfinity.png
---

## Cloud Sanity Check
The goal of this challenge is to identify and exploit misconfigurations in AWS services to retrieve a hidden flag. Use the credentials below to access the resources.

![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image1.png)
_AWS Credentials_

## Listing AWS IAM Users
I began by attempting to enumerate available AWS resources using the provided credentials. My initial focus was on listing IAM users, S3 buckets, and Lambda functions to identify potential avenues for privilege escalation or data retrieval.


![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image2.png)
_List IAM Users_
``` terminal
aws iam list-users
```
After executing the command, I got an error (AccessDenied). It says there that I don’t have the permission to list IAM users. No Identity-based policy allows the iam:ListUsers action. Since listing IAM users was restricted, I pivoted to enumerating other services that might reveal sensitive information.

Same thing happened when I checked the services I’m allowed to interact with using the command below:

```terminal
aws service-quotas list-services
```

This gives me an idea of what’s available, which could help me figure out where to look for possible vulnerabilities or misconfigurations. But, unfortunately, still no luck at all.

## Checking AWS S3
Since I don’t have permissions to list IAM users, I now wanted to check S3 buckets if there are publicly available bucket to exploit.

![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image3.png)
_List S3 Buckets_
```terminal
aws s3 ls
```

Still, with no luck, I got an error (AccessDenied). This is because I do not have the permission to list S3 buckets. The user does not have an identity-based policy that allows the s3:ListAllMyBuckets action. After encountering a permission error, I decided to check for Lambda functions that might be accessible.

## Listing Lambda functions
I attempted to list lambda functions, but yeah, I do not have permissions for this.

![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image4.png)
_List Lambda Functions_

## AWS Secrets Manager
As I was browsing what are the Key AWS Services, I found an interesting service called SercretsManager. I read its documentation and thought this service might have the flag I am looking for. Using the command below:
```terminal
aws secretsmanager list-secrets
```
![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image5.png)
_List Secrets_

And there we go!!! I finally found the flag. Easy Peasy Lemon Squeezy

![Desktop View](/assets/img/posts/2025-06-23-THM-Hackfinity-Cloud-1/image6.png)
_{Flag Found}_
