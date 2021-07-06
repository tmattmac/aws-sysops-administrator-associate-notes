# EC2 Storage and Data Management (EBS/EFS)

## EBS Volumes

* Attached network volume that persists between terminations
* Can be detached and reattached to instances easily
* Locked to individual AZ, but can be copied via snapshot
* Provisioned capacity (size and IOPS) - linked to cost

## EBS Volume Types

* GP2 - general purpose SSD
  * Recommended for most purposes
  * 1GB to 16TB
  * 3 IOPS per GB
  * Minimum 100 IOPS, maximum 16,000
  * Burstable to 3,000 if below 3,000
* IO1 - highest performance SSD, low latency, high throughput
  * Recommended for critical applications requiring sustained IOPS performance or more than 16,000 IOPS per volume
  * 4GB to 16TB
  * IOPS provisioned directly, 100 to 32,000 (64,000 for Nitro instances)
  * 50 IOPS per 1GB maximum
* ST1 - very high throughput HDD, lower cost
  * Useful for big streaming data
  * 500GB to 16TB
  * Max IOPS is 500
  * Max throughput is 500 MB/s
* SC1 - cold storage HDD
  * Useful for high-throughput, infrequently accessed data
  * 500GB to 16TB
  * Max IOPS is 250
  * Max throughput of 250 MB/s, burstable
* Only SSDs can be used as boot volume

## EBS Operations

* Resizing
  * Can only increase size
  * Can increase IOPS for IO1 volume
  * Drive must be repartitioned to use new space
  * Performance may be impacted while transitioning to the larger size
* Snapshots
  * Incremental - only backs up changed blocks
  * Uses IO, don't snapshot while handling heavy traffic
  * Stored in S3
  * Recommended to detach volume before snapshot
  * Can specify schedule, retention period, and number of retained snapshots with lifecycle policy
* Volume Migration Process
  * Snapshot
  * Copy to new region
  * Create volume from snapshot in new region
* Encryption (optional)
  * Data is encrypted at rest
  * Data encrypted in flight between instance and volume
  * Snapshots and volumes from snapshots are encrypted
  * Encryption and decryption happen transparently
  * Minimal impact on latency
  * To encrypt an unencrypted volume
    * Create a new snapshot
    * Copy the snapshot with encryption
    * Create a volume from the encrypted snapshot
* RAID
  * OS-level configuration of EBS volumes
  * RAID 0 (performance) - two volumes as one logical volume, use total space and I/O, higher failure chance
  * RAID 1 (fault tolerance) - mirror two volumes, write to both at same time, higher network throughput
  * RAID 5 and RAID 6 not recommended for EBS

## EBS Monitoring

* Important CloudWatch metrics
  * VolumeIdleTime
  * VolumeQueueLength - queued disk operations
  * BurstBalance - IOPS burst balance
* Metrics sent every 5 minutes for GP2, every minute for IO1

# EFS

* Managed network file system (NFS) mountable on many EC2 instances simultaneously
* Multi-AZ, highly available and scalable
* Very expensive
* NFSv4.1 protocol
* Control access with security groups
* Only compatible with Linux AMIs
* Supports encryption at rest
* Performance modes
  * General purpose - low latency
  * Max I/O - higher latency, higher throughput, highly parallel
* Storage Tiers - lifecycle management (like S3)
  * Standard
  * Infrequent access (EFS-IA) - cheaper storage, but pay to retrieve files
