# CloudFormation

SysOps-specific topics are covered in this section.

## EC2 User Data

* EC2 instance user data can be provided in CloudFormation
  * `UserData` property value
  * Must convert to base64 using `Fn::Base64`
  * Script log located at `/var/log/cloud-init-output.log`
* Can use `cfn-init` script to simplify complex configuration
  * Add `AWS::CloudFormation::Init` to `Metadata` key of resource
  * Run `cfn-init` command in user data section
  * EC2 fetches data from CloudFormation service
  * Logs located at `/var/log/cfn-init.log`
* Can use `cfn-signal` script to fail or continue stack creation if `cfn-init` script fails
  * Define `WaitCondition` to pause stack creation until signal is received from `cfn-signal`
  * Attach `CreationPolicy` property with a `ResourceSignal` subproperty; can define time to wait
  * Run `cfn-signal` in user data, referencing the wait condition to signal
  * Must disable CloudFormation rollbacks to debug via SSH

## Nested Stacks

* Can reference stack within another stack
  * `AWS::CloudFormation::Stack` type with `TemplateUrl` defined in `Properties`
  * Can pass values to stack using `Parameters` property
  * Can reference stack outputs
* Useful for common configurations (VPCs, load balancers, security groups)
* Update nested stack by updating the parent stack

## Change sets

* Show the changes made to resources in a stack when updating the stack
* Don't say if the update will succeed
* Change set can be saved and applied later

## Policies

* Can retain resources when a template is deleted with `DeletionPolicy`
  * `Retain` preserves or backs up the resource
  * `Snapshot` creates a snapshot of certain resources (DBs and EBS volumes)
  * `Delete` is the default behavior
* Can use `CreationPolicy` to wait for a signal before marking a resource as created (see additional notes in **EC2 User Data** section)
* `UpdatePolicy` lets you specify what should be updated when a resource in a stack is updated
  * Applicable for ASG, Lambda Alias, and ElasticCache Replication Groups
  * `UpdatePolicy` for ASGs has three important types
    * `AutoScalingReplacingUpdate`
      * Trigger when updating launch configuration or VPC zones
      * Create new ASG with new instances, then delete old one
    * `AutoScalingRollingUpdate`
      * Trigger when updating launch configuration or VPC zones
      * Replace instances in specified batches
    * `AutoScalingScheduledAction`
      * Lets you prevent ASG scheduled actions from modifying capacity values when updating

## Miscellaneous

* Can enable termination protection to prevent stack from being deleted
