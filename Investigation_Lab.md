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

<img width="615" height="199" alt="image" src="https://github.com/user-attachments/assets/dc0e67aa-6780-4020-bfbd-c66805ba63da" />

<img width="618" height="127" alt="image" src="https://github.com/user-attachments/assets/14fe3c0a-f3ca-4202-94fc-5114e20fcab0" />

<img width="611" height="251" alt="image" src="https://github.com/user-attachments/assets/33f5922c-eb3d-49f8-b363-3c9981fb6228" />

<img width="618" height="127" alt="image" src="https://github.com/user-attachments/assets/0e392d06-c1a5-4121-a283-4511985e5a50" />

<img width="615" height="216" alt="image" src="https://github.com/user-attachments/assets/2f48103b-b171-43ca-9f2b-a5ce565092d7" />

<img width="609" height="121" alt="image" src="https://github.com/user-attachments/assets/95bb7bf8-be1f-4e79-b282-6223dff476d2" />

<img width="599" height="178" alt="image" src="https://github.com/user-attachments/assets/e4dacdd1-aa7c-4d14-a8a0-eed79d7a5992" />

<img width="603" height="99" alt="image" src="https://github.com/user-attachments/assets/bd9b1312-07a9-4674-890b-29f366626a1e" />


---
