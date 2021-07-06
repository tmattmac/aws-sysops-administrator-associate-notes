# Monitoring and Auditing

## CloudWatch Metrics

* Refers to a variable to monitor
* Belong to namespaces
* Has dimensions which represent attributes (up to 10)
* Can create dashboard from metrics
  * Dashboards are global and can include graphs from different regions
  * Can set up automatic refresh
* Can use PutMetricData API to send custom metrics
  * Can segment with dimensions
  * Standard one-minute resolution, up to one second
  * Requires CloudWatch agent

## CloudWatch Logs

* Logs automatically created from certain AWS services
* Can be exported to ElasticSearch or S3
* Requires log group, which contains log stream
* Can define log expiration policies
* Requires correct IAM roles
* Can send custom logs to CloudWatch using CloudWatch agent
* Can create custom metrics using log filters

## CloudWatch Alarms

* Trigger alarms based on metrics
* OK, INSUFFICIENT_DATA, and ALARM states
* Alarms can go to ASGs, EC2 actions, SNS notifications

## CloudWatch Events (EventBridge)

* Based on a source and a rule, send an event to a target
* Events can be triggered by a schedule or event pattern (AWS service change)
* Sends JSON with event data to Lambda or SQS/SNS/Kinesis Messages

## CloudTrail

* Logging of management events is done by default
* Can optionally configure logging for data events (e.g. S3 and Lambda operations)
* Can use CloudTrail Insights to identify unusual activity on your account
* Events are stored for 90 days, but can be saved for longer to S3

## AWS Config

* Record compliance and configuration over time
* Can store data in S3
* Region-locked
* Can use AWS-managed rules or custom rules using Lambda
* Can be triggered for either each config change or at regular intervals
