---
name: cloudwatch-alarms
description: Set up CloudWatch monitoring and alarms
category: cloud
tags: ["cloud", "aws", "monitoring"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# CloudWatch Alarms

> Set up CloudWatch monitoring and alarms

You are an AWS CloudWatch expert. The user wants to set up CloudWatch monitoring and alarms to track metrics, detect anomalies, and trigger notifications.

## What to check first
- Verify AWS CLI is installed and configured: `aws sts get-caller-identity`
- Confirm IAM permissions include `cloudwatch:PutMetricAlarm`, `cloudwatch:PutMetricData`, and `sns:Publish`
- Check if SNS topic exists for notifications: `aws sns list-topics`

## Steps
1. Create an SNS topic for alarm notifications using `aws sns create-topic --name cloudwatch-alarms`
2. Subscribe your email to the SNS topic: `aws sns subscribe --topic-arn arn:aws:sns:region:account:cloudwatch-alarms --protocol email --notification-endpoint your-email@example.com`
3. Confirm the SNS subscription by clicking the email link AWS sends
4. Identify the metric namespace and dimensions for your resource (e.g., `AWS/EC2` for EC2 instances)
5. Create a threshold alarm using `PutMetricAlarm` that triggers when a metric exceeds a statistic over a specified period
6. Set the `ComparisonOperator` (GreaterThanThreshold, LessThanThreshold, etc.) and `EvaluationPeriods` (consecutive periods before alarm triggers)
7. Bind the alarm to your SNS topic in the `AlarmActions` parameter so notifications are sent when the alarm state changes
8. Test the alarm by publishing a test metric: `aws cloudwatch put-metric-data --metric-name TestMetric --value 100`

## Code
```python
import boto3
from datetime import datetime

cloudwatch = boto3.client('cloudwatch')
sns = boto3.client('sns')

# Step 1: Create SNS topic (if not exists)
topic_response = sns.create_topic(Name='cloudwatch-alarms')
topic_arn = topic_response['TopicArn']
print(f"SNS Topic ARN: {topic_arn}")

# Step 2: Create a high CPU alarm for EC2 instance
cloudwatch.put_metric_alarm(
    AlarmName='High-CPU-Usage-Production',
    ComparisonOperator='GreaterThanThreshold',
    EvaluationPeriods=2,
    MetricName='CPUUtilization',
    Namespace='AWS/EC2',
    Period=300,  # 5 minutes
    Statistic='Average',
    Threshold=80.0,
    ActionsEnabled=True,
    AlarmActions=[topic_arn],
    AlarmDescription='Alert when EC2 CPU exceeds 80% for 10 minutes',
    Dimensions=[
        {
            'Name': 'InstanceId',
            'Value': 'i-0123456789abcdef0'  # Replace with your instance ID
        }
    ]
)
print("CPU
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

