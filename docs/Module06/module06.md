# EC2 instances storages

## EBS - Elastic Block Store

- It's a virtual HD (volume) you can attach to the instance of EC2, for example;
- Network drive: it's a network drive that connects via network to the instance;
- The data persists even after terminating the instance;
- Created for a **specific AZ**. If you want to move to another AZ, you need to create a snapshot;
- Generally used with EC2 for boot volumes and data volumes;

> Nowadays we use other services to store the data and do not need to create a data volume for that:

- S3: Static files, backups, data lakes
- RDS/Aurora: Managed databases
- EFS: Shared file system
- DynamoDB: Managed NoSQL
- ElastiCache: In-memory cache


**But, in some cases it is necessary:**

- The application has not been modernized yet: legacy apps with filesystem local;
- Team has migration restrictions: compliance, budget, time
- Fine control: specific performance or custom configurations;



➝ Know the main types os storages:

1. gp3 (General Purpose): default, balanced (good cost/perf).

2. io1/io2 (Provisioned IOPS): high performance, critical databases.

3. st1 (Throughput Optimized HDD): cheap, large volume, high throughput (e.g., logs).

4. sc1 (Cold HDD): even cheaper, infrequently accessed data.



## Instance Store (EC2 Local Storage)

- **Physical storage** attached to the EC2 host
- **Temporary**: Data lost when instance stops/terminates
- **High performance**: Very low latency, high IOPS
- **Free**: Included with instance type
- **Use cases**: Cache, temporary processing, high-performance databases

**Key differences:**
- EBS = Network drive (persistent)
- Instance Store = Physical drive (temporary)


## S3 / EFS / RDS / DynamoDB

Yes, they are NOT EBS, but they appear as comparison.

Typical question: “Where to persist data for multiple EC2 instances?”
➝ Answer: EFS or S3, not EBS (because EBS only attaches to one instance at a time, except io1/io2 Multi-Attach in specific cases). 


### EFS
- é um systema de arquivo que é Multi-AZ

### EBS Multi-Attach x EFS/FSx
- EBS Multi-Attach = only when the application is cluster-aware and knows how to handle block concurrency.

- EFS/FSx = when you need ready and easy file sharing between instances. (Knows how to handle concurrency and blocking)

---

## Metrics: IOPS x Throughput

**IOPS** = Input/Output Operations Per Second: It's a metric that measures how many read/write operations a volume can perform per second.

- How many times you access the disk
- Important for: frequent small operations
- Example: 1000 SQL queries per second

**Throughput** = amount of data transferred per second - MB/s;

- How much data you transfer per second
- Important for: transferring large files
- Example: Transfer 500 MB/s of video

**Types of usage with EC2:**

| Root Volume (Boot) | Data Volumes |
|-------------------|--------------|
| • Operating System | • Applications |
| • Basic configurations | • Databases |
| • Usually gp2/gp3 | • User data/files |
| • DeleteOnTermination = `true` | • Usually gp2/gp3 or others |
|  | • DeleteOnTermination = `false` |


## Quiz:

1. EBS Volumes are created for specific AZs. If you want to migrate to another, it will require a snapshot.

2. An instance has the Root Volume (AMI and instance configs) and the Volume with the data we put.

Root volume = disposable infrastructure (`DeleteOnTermination = true`)

Data volume = persistent data (`DeleteOnTermination = false`)

When we terminate an instance, the root volume is deleted. The data volume is not.

3. Which of the following EBS volume types can be used as boot volumes when you create EC2 instances?

SSD = can boot (gp2, gp3, io1, io2).

HDD = only data (st1, sc1, standard).


4. A company wants to reduce costs and use st1 for their root volume. What will happen?

st1 cannot be a root volume, only data volume.

5. Which EBS type is best for boot volumes requiring high IOPS?
io1 or io2

Note: st1 (cheap, but only for throughput, not boot)

6. What is EBS Multi-Attach?

It's only for data volumes (not root/boot). Up to 16 instances can share the same data volume as long as they are in the same AZ. Attention: unlike EFS or FSx, EBS is not a "managed shared file system". Consistency is the application's responsibility.


- AMI:

1. You can use an AMI in N.Virginia Region us-east-1 to launch an EC2 instance in any AWS Region?

You cannot use an AMI in N. Virginia (us-east-1) to launch an EC2 instance in another AWS Region directly. An AMI only points to the EBS snapshots that are used to recreate volumes, and those snapshots exist only in the region where they were created (stored in S3 regionally). Therefore, if you want to launch the same AMI in another region, you must copy the AMI to the destination region, which copies the snapshots as well.


