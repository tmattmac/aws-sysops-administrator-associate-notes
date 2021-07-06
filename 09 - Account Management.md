# Account Management

## AWS Organizations

* Lets you manage multiple member accounts under a single master account
* Member accounts can only be mapped to one organization
* Allows for consolidated billing across accounts
* Can send CloudTrail or CloudWatch logs to a central account
* Organized into organizational units, which can be nested
* Can use Service Control Policies (SCP) to blanket allow/deny IAM actions at the account or OU level
  * Permissions are inherited by lower-level OUs
  * Does not affect master account
  * Does not affect service roles

## Miscellaneous

* Check the service health dashboard to know if certain AWS services or regions are down, as well as historical data
* Can use personal health dashboard to see if any outages are impacting your resources directly and what can be done to fix them