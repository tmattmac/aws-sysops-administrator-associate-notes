# SSM & OpsWorks

## Systems Manager Overview

* Manage EC2 and on-premise systems at scale
* Automation of patching
* Resource Groups for granular control of infrastructure using tags (or CF stack)
* Store parameters that can be accessed by instances
* Requires SSM agent (pre-installed on Amazon Linux AMI) and SSM role for EC2

## SSM Documents
* JSON/YAML script with parameters and actions
* Used to define automations and commands
* Useful AWS-provided document is AWSSupport-ResetAccess, which lets you refresh SSH keys

## Run Command
* Run document or command across multiple instances
* SSH not required

## Session Manager
* Connect to instance with SSM agent
* Actions done through shell are logged to S3 or CloudWatch (permission necessary)
* No need for bastion hosts or port 22 to be open
* Uses user IAM credentials rather than SSH keys

## Parameter Store
* Securely store and version configuration and secrets
* Stored in a hierarchy
* Can enable seamless encryption with KMS
  * User/role requesting parameter requires KMS key permissions
  * Decryption process happens seamlessly (`--with-decryption` flag in CLI)
* IAM permissions for parameters
* EventBridge and CloudFormation integration
* Commands
  * `get-parameters` - get specified list of parameters
  * `get-parameters-by-path` - get all parameters under path, use `--recursive` flag for subpaths

## Other Tools
* Inventory - list software installed on instance
* Patch Manager - OS patching
* Compliance - check that patches have been applied

## OpsWorks
* Managed Chef and Puppet
* Configuration as code (similar to SSM)
* 