---
name: s3-operations
description: Set up S3 bucket operations (upload, download, presigned URLs)
category: cloud
tags: ["cloud", "aws", "s3"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# S3 Operations

> Set up S3 bucket operations (upload, download, presigned URLs)

You are an AWS developer setting up S3 bucket operations. The user wants to upload files, download files, and generate presigned URLs for temporary access.

## What to check first
- Run `aws configure` to verify AWS credentials are set (Access Key ID, Secret Access Key, region)
- Run `aws s3 ls` to confirm S3 bucket access and list available buckets
- Verify the bucket name and region match your AWS account setup

## Steps
1. Install boto3 using `pip install boto3` — this is the official AWS SDK for Python
2. Create an S3 client with `boto3.client('s3')` specifying your region
3. For uploads, use `put_object()` method with Bucket, Key, and Body parameters
4. For downloads, use `get_object()` method and read the StreamingBody response
5. For presigned URLs, use `generate_presigned_url()` with ClientMethod, Params, and ExpiresIn (seconds)
6. Set ExpiresIn to control URL expiration time (3600 = 1 hour, 86400 = 1 day)
7. Handle exceptions like NoCredentialsError, ClientError, and FileNotFoundError
8. Test each operation with a small test file before scaling to production

## Code
```python
import boto3
from botocore.exceptions import ClientError, NoCredentialsError
import os

class S3Operations:
    def __init__(self, bucket_name, region_name='us-east-1'):
        """Initialize S3 client with bucket name and region"""
        try:
            self.s3_client = boto3.client('s3', region_name=region_name)
            self.bucket_name = bucket_name
        except NoCredentialsError:
            print("ERROR: AWS credentials not configured")
            raise

    def upload_file(self, file_path, s3_key=None):
        """Upload file to S3 bucket"""
        if not os.path.exists(file_path):
            raise FileNotFoundError(f"File {file_path} not found")
        
        s3_key = s3_key or os.path.basename(file_path)
        
        try:
            self.s3_client.put_object(
                Bucket=self.bucket_name,
                Key=s3_key,
                Body=open(file_path, 'rb')
            )
            print(f"✓ Uploaded {file_path} to s3://{self.bucket_name}/{s3_key}")
            return f"s3://{self.bucket_name}/{s3_key}"
        except ClientError as e:
            print(f"ERROR uploading file: {e}")
            raise

    def download_file(self, s3_key, local_path):
        """Download file from S3 bucket"""
        try:
            response = self.s3_client.get_object(
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

