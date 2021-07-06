# S3 for SysOps

## Versioning

* Encrypting a file in versioned bucket creates a new version
* Deleting a versioned file just adds a delete marker; bucket deletion requires removing all file versions

## MFA Delete

* Bucket must be versioned
* Requires MFA to permanently delete file version or suspend versioning
* Only bucket owner can configure MFA Delete
* Can only be enabled with CLI

## S3 Access Logs

* Must enable server access logging on bucket
* Logs should be saved to another bucket to avoid infinite logging loop

## Replication

* Versioning must be enabled on both buckets
* Same-region and cross-region replication possible
* Buckets can be in different accounts
* Copying is asynchronous and requires IAM access to second bucket
* Not retroactive after activating
* Can't chain replications to multiple buckets
* Can choose to replicate delete markers
* Permenant version deletion is not replicated

## CloudFront

* Basics to Remember
  * Different origins: S3, EC2, ALB, any HTTP endpoint
  * Origin Access Identity for S3 to only allow bucket access from CloudFront
  * Geo Restriction based on GeoIP database
* Monitoring
  * CloudFront access logs available (similar to S3 access logs)
  * Reports on things like usage and popular objects can be created based on access log data

## S3 Inventory

* Audit and report encryption and replication status
* Useful for compliance and regulatory needs
* Data goes from source to target bucket
* Can create multiple inventories
* Output to CSV or other formats

## Snowball

* Physically transfer up to 10PB of data in or out of S3
* Use when network transfer is prohibitively slow
* Snowball Edge adds compute power to device, useful for processing data, 100TB maximum size
* Snowmobile supports a maximum of 100PB (more with parallel trucks), useful for exabyte-scale data
* Can't import from Snowball to Glacier directly, must use lifecycle policy

## Storage Gateway

* Bridge between on-premise data and cloud data in S3
* Types
  * File Gateway - S3 bucket accessible via NFS or SMB
  * Volume Gateway - iSCSI block storage backed by EBS snapshots
  * Tape Gateway - use virtual tape library (VTL) to integrate tape-based processes

## Miscellaneous

* S3 has a very high API rate limit, but calls to encrypted objects may be bottlenecked by KMS rate limits
* S3/Glacier Select lets you use simple SQL to filter rows and columns of data on files in S3 server-side
* Can create event notification rules to trigger SNS, SQS, or Lambda
* S3 Analytics lets you create a report to help optimize storage classes and lifecycle rules
* Glacier vaults have a vault access policy (similar to bucket policies) and a vault lock policy (immutable policy for compliance requirements)
* Athena is a serverless service allowing you to run queries directly against S3 buckets using SQL
