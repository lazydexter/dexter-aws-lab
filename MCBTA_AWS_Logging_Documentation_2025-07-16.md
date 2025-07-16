
# 🛡️ MCBTA: AWS Logging and Monitoring Lab Setup

This lab demonstrates end-to-end AWS log ingestion using CloudTrail → S3 → SQS → Filebeat → ELK for threat detection and monitoring.

---

## 🌐 Architecture Overview

- **CloudTrail** logs AWS API activity
- **S3 Bucket** stores CloudTrail logs
- **SQS Queue** receives S3 event notifications
- **Filebeat** reads SQS messages and fetches logs
- **Kibana** used for log visibility and analysis

---

## 🔧 Step-by-Step Setup

### 1. AWS CloudTrail Setup

- Region: `eu-north-1`
- S3 Bucket: `mcbta-cloudtrail-logs-dexter`
- Enabled Management, Data, and Insights events

### 2. S3 Bucket Setup

- Object ownership: Bucket owner enforced
- Block Public Access: Enabled (all)
- Versioning: Enabled
- Encryption: SSE-S3

### 3. SQS Setup

- Queue name: `cloudtrail-log-queue`
- Type: Standard
- Access Policy (edited):

```json
{
  "Version": "2012-10-17",
  "Id": "CloudTrailS3ToSQS",
  "Statement": [
    {
      "Sid": "AllowS3BucketToSendToSQS",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "SQS:SendMessage",
      "Resource": "arn:aws:sqs:eu-north-1:538189757084:cloudtrail-log-queue",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "538189757084"
        },
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:::mcbta-cloudtrail-logs-dexter"
        }
      }
    }
  ]
}
```

### 4. IAM User (admin-mcbta)

- Permissions: `AdministratorAccess`, `IAMUserChangePassword`
- Access key and secret key generated

---

## 🖥️ EC2 Instance for Filebeat

- OS: Ubuntu Server 22.04 LTS
- Instance type: `t3.medium`
- Inbound: SSH (22), Outbound: all
- Key Pair: `mcbta-key.pem`

---

## 📦 Filebeat Setup

- Filebeat installed via `.deb` package
- AWS module enabled:

```bash
filebeat modules enable aws
```

- Config (`/etc/filebeat/modules.d/aws.yml`):

```yaml
- module: aws
  cloudtrail:
    enabled: true
    var.queue_url: https://sqs.eu-north-1.amazonaws.com/538189757084/cloudtrail-log-queue
    var.access_key_id: YOUR_ACCESS_KEY
    var.secret_access_key: YOUR_SECRET_KEY
```

- Restarted Filebeat:

```bash
sudo systemctl restart filebeat
```

---

## ✅ Validation

- Logs visible in **Kibana > Discover > filebeat-***.
- Events include CloudTrail log entries.

---

## 📸 Screenshots (To Be Added)

- ✅ AWS CloudTrail config
- ✅ S3 bucket settings
- ✅ SQS access policy
- ✅ Filebeat config
- ✅ Kibana log visibility

---

## 🧠 Key Learning

This setup demonstrates how to monitor AWS accounts by forwarding API activity logs into a SIEM solution for real-time visibility and threat detection.

---
