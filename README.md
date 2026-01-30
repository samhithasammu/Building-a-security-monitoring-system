# Build a Security Monitoring System

**Author:** samhithabbb@gmail.com  
**Email:** samhithabbb@gmail.com

---

![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

In this project, I demonstrated how to build a security monitoring system in AWS that detects and alerts when a sensitive secret is accessed.
I’m doing this project to learn how AWS security services work together to provide visibility, monitoring, and real-time notifications for suspicious or sensitive activity.

### Tools and concepts

Services I used were:
1. AWS Secrets Manager
2. AWS CloudTrail
3. Amazon CloudWatch (Logs, Metrics, Alarms)
4. Amazon SNS
5. AWS IAM
6. Amazon S3                                                                                                                                  
Key concepts I learnt include:
1. Securely storing secrets using encryption
2. Logging AWS API activity using CloudTrail
3. Differentiating Read vs Write API events
4. Sending CloudTrail logs to CloudWatch Logs
5. Creating metric filters from logs
6. Triggering alarms and notifications using SNS


### Project reflection

This project took me approximately 4–6 hours to complete, including troubleshooting.
The most challenging part was figuring out why the CloudWatch alarm wasn’t sending emails, even though the secret was being accessed.
The most rewarding part was finally receiving the alert email, knowing the entire monitoring system was working end-to-end.

---

## Create a Secret

Secrets Manager is an AWS service that helps securely store and manage sensitive information like passwords, API keys, and credentials.
You could use Secrets Manager to avoid hardcoding secrets in applications and reduce the risk of data leaks.

To set up my project, I created a secret called TopSecretInfo that contains a simple key-value pair used only for testing and monitoring purposes

![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

CloudTrail is an AWS service that records all API activity in an AWS account.
I set up a trail to track management events, especially actions related to Secrets Manager.

CloudTrail events include types like:
1. Management events
2. Data events
3. Insights events

For this project, management events were required because retrieving a secret is logged as a management API call.



### Read vs Write Activity

Read API activity involves viewing metadata or listing resources without changing them.
Write API activity involves creating, modifying, deleting resources, or retrieving sensitive values.

For this project, we need Write API activity, because retrieving a secret value (GetSecretValue) is classified as a write operation.

---

## Verifying CloudTrail

I retrieved the secret in two ways:
1. First through the AWS Management Console
2. Second using the AWS CLI via CloudShell

To analyze my CloudTrail events, I visited CloudTrail Event History and filtered by secretsmanager.amazonaws.com.
I found multiple GetSecretValue events, which tells me CloudTrail is correctly logging secret access.



![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

CloudWatch Logs is a service that stores and analyzes logs from AWS services.
It’s important for monitoring because it allows real-time analysis, filtering, and alerting.

CloudTrail’s Event History is useful for short-term investigations, while CloudWatch Logs are better for long-term monitoring and alarms.

A CloudWatch metric is a numerical data point generated from logs.
When setting up a metric, the metric value represents how many times a specific event occurs.
The default value is used when no matching event occurs, ensuring continuous visibility.A CloudWatch metric is... When setting up a metric, the metric value represents... Default value is used when...

![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

A CloudWatch alarm is a rule that monitors metrics and triggers actions when thresholds are crossed.
I set my CloudWatch alarm threshold to trigger when the metric is greater than or equal to 1, meaning the secret was accessed.

I created an SNS topic along the way.
An SNS topic is a messaging channel used to send notifications.
My SNS topic is set up to send email alerts whenever the alarm enters the ALARM state.

AWS requires email confirmation because it prevents unauthorized or accidental email subscriptions, ensuring alerts go to valid recipients.





![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

To test my monitoring system, I accessed the secret multiple times.
Initially, I did not receive any email alerts.

When troubleshooting the notification issues, I checked:
1. CloudTrail event history
2. CloudWatch log delivery
3. Metric filter accuracy
4. Alarm configuration
5. SNS subscription status

I initially didn’t receive an email because the alarm statistic was set to Average instead of Sum.
The key solution was changing the statistic to Sum, allowing the alarm to trigger correctly.





---

## Success!

To validate the system, I retrieved the secret again.
I checked the CloudWatch alarm state and confirmed it entered ALARM.
I received an email notification, confirming the monitoring system works as expected.

![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

In a project extension, I enabled direct SNS notifications from CloudTrail to compare approaches.
After enabling CloudTrail SNS notifications, my inbox received many emails, even for unrelated account activity.

In terms of usefulness, I found:
1. CloudTrail SNS notifications are noisy and best for automated systems
2. CloudWatch alarms are targeted and better for human-readable alerts

This comparison helped me understand real-world cloud architecture decisions.



![Image](http://learn.nextwork.org/motivated_cyan_peaceful_tiger/uploads/aws-security-monitoring_d7e8f9g0)

---

---
