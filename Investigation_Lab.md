# AWS CloudTrail Investigations

This repository documents a set of threat investigations performed using CloudTrail logs ingested via Filebeat into Kibana. It includes detection of suspicious activity involving S3 bucket deletions, Lambda function creations, access key generation, and more.

## Investigated Scenarios

1. **S3 Bucket Enumeration and Deletion**
   - Public bucket: `devopspipeline4586`
   - Detected enumeration via `HeadBucket` and `GetObject`
   - Discovered object: `env.txt`
   - Deleted bucket: `appbackupfilesbuk`

2. **IAM Abuse Detection**
   - Access key created for user: `arn:aws:sts::010928207857:assumed-role/Priveleged_Role/General_Audit`
   - Instance profile created: `CreateInstanceProfile`
   - IAM role added to profile: `AddRoleToInstanceProfile`

3. **Lambda Function Abuse**
   - Lambda function created via `CreateFunction`
   - Function name extracted from `request_parameters`

4. **Secrets Manager Access**
   - Accessed secret: `AppServerSecret`
   - Action: `GetSecretValue` by same privileged role

5. **CloudTrail Stop Logging**
   - Attempted `StopLogging` captured via event.action filter

## Sample Queries

```kql
aws.cloudtrail.request_parameters : *devopspipeline4586* and event.action : "GetObject" and event.outcome : "success"
event.action : "CreateAccessKey"
event.action : "CreateInstanceProfile"
event.action : "AddRoleToInstanceProfile"
event.action : "GetSecretValue"
event.action : "StopLogging"
```

## Screenshots

All screenshots used in this lab are available in the `screenshots/` directory.


---
