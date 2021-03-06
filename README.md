# AWS Certified Developer Prep

> This repo will be used as I prepare for my AWS Certified Developer certification. I will be committing all notes and code I write over the course of my preparation.
* I have actually provisioned my first fully self-deployed webserver in order to apply some of this knowledge.
> www.louiswillcock.com

* I am using Udemy to prepare.
> https://www.udemy.com/aws-certified-developer-associate

## EC2

* EC2 gives you access to VMs on demand, with which you can run whatever you wish, they come in varying capacities and sizes. Scaling in price.

#### Pricing

* On Demand - Whenever needed
> Testing of ideas, only need a server for an hour or two at random times.
* Reserved  - Capacity reservation for period of time.
> Applications with steady and predictable use.
* Spot 	    - Bid on excess capacity.
> Predictable but infrequent use. Genomics companies need massive capacity that would be expensive, spot allows them to bid and schedule their apps to run at strange times.
* Dedicated - Normal dedicated hosting.
> When you are mandated for use of physical servers and have predictable use.

* Exam Tip
> If the spot instance is terminated by Amazon, you will not be charged for a partial hour of usage. However, if you teriminate the instance yourself, you will be charged for any hour in which the instance ran.

#### EC2 Instance Types

* EC2 Instances are separated into families with different uses.

* D2 - Dense Storage - Fileservers/DataWarehousing/Hadoop
* R4 - RAM Optimised - Memory Intensive Apps/DBs
* M4 - Main Instance - General purpose App Servers.
* C4 - Compute Optimised - CPU Intensive Apps/DBs
* G2 - Graphics Intensive - Vid Encoding/3D App Streaming
* I2 - IOperations - NoSQL DBs, Data WareHousing
* F1 - Field Programmable Gate Array - Hardware Accel for Code
* T2 - Trivial Tasks - Low Cost/Gen Purpose Web Servers/Small DBs
* P2 - Powerful Graphics - ML/Bitcoin Mining
* X1 - Extreme RAM - SAP HANA/Apache Spark

>DIRTMCGX

## EBS - Elastic Block Store

* EBS creates storage volumes that can be attached to EC2 instances in the same way you would add a HDD to a computer. They are auto replicated to avoid failure and data corruption.

#### EBS Types

* GP2 - General Purpose (SSD)
> 3 IOPS per GB with upto 10,000 IOPS per volume.
* IO1 - Provisioned IOPS (SSD)
> NoSQL and I/O intensive apps. Use if you need >10,000IOPS. Upto 20,000IOPS per volume.
* ST1 - Throughput Optimised (HDD)
> Big Data, Data WareHousing, Log Processing. Can't be a boot volume.
* SC1 - Cold Storage (HDD)
> Lowest cost storage. Infrequently accessed. File servers. Can't be a boot volume.
* Magnetic Standard (HDD)
> Lowest cost bootable drive. Applications where lower storage cost is importnat.

### EC2 Exam Tips

* Know the differences between pricing tiers.

* Remember about teriminating spot instances.

* Understand differences of EBS types.

* You can't mount 1 EBS volume to multiple EC2 instances, use EFS instead.

* DRMCGIFTPX - EC2 Instance Types

## Configuring instances

#### Subnets, VPC and Avail.Zones.

* Subnets can only be used for one availability zone, ie: sydney-1a or sydney-1b. You can't have subnets across multiple availability zones.

#### Tags
* Tags should be used in as fine grain way possible. They allow labelling of instances and location of costs in bigger organisations. You use tags to highlight the employee procuring the service, or department budget to charge to.

#### Security Groups
* Security Groups are effectively firewalls that we control.


> Setting up the apache server was actually ridiculously easy. Wow. Superb. Will get my website hosted by the end of the night most likely.

### Exam Tips

* Termination Protection is turned off by default, you must turn it on.

* On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated.

* Default AMI root volumes cannot be encrypted, you must create an image of a built VM, then encrypt that. Or you can use bitlocker.

## Security Groups

* A security group is just a virtual firewall.

* 1 instance can have multiple security groups. The benefit of this is that you can create boilerplate rulesets for different applications, this allows us to leverage effective mental modelling. We know what a standard DB instance's rules will look like, and what a applications rules too.

* Changes to security group (firewall) rules, are effected immediately, a change to http inbound will be felt straight away, most likely in the form of a timeout.

* Inbound/Outbound is stateful, in that, if you have inbound http rules, you don't require outbound http rules, it will automatically allow traffic back out.

* AWS EC2 instances implement a "trust no one, authenticate few" model of security. In that, by default, ports and protocols are treated as malicious, and are required to be given access case by case. A great way of doing security.

* If you wish to block IP addresses, you cannot do it using SecGroups, you do it with Network Access Control Lists.

## Volumes VS Snapshots

* When first building an instance and you get to the select EBS or Volume, some choices won't be available. After this, if you decide to add a volume, they will become available. These are the non-root volumes.

* Cannot attach volumes from outside the availablity zone of your instance.
> IE: If you're instance is in Chicago1A you can't attach a volume from Chicago1B.

* There is a long process with a few steps involved in creating a new volume to add onto an AWS instance. You must provision it from AWS. Attach it to an instance. Then SSH into the instance. From here, run lsblk to find the thing. cd into the / root dir. mkdir whatever you want to call it, myfileserver is a good example.
Then from the / root dir. run mkfs -t ext4 /dev/xvdf or the fileextension of the volume.

* umount to unmount, then unassociate it from AWS. You can then make a snapshot of the volume, which will hold all the data on the volume. Which you can spin up whenever you need.

* Volumes exist on EBS. They are virtual hard disks.

* Snapshots exist on S3.

* Snapshots are point in time copies of Volumes.

* Snapshots are incremental. Only records changes.

## EC2 User-Roles

* Use roles instead of storing your access key and secret access key on individual instances. This is because these values are stored in plain text config files, should your ssh keys be stolen or you otherwise lose access. You are pretty much fucked.

* Roles are your friend, much easier to handle and manage than the aws configure command.

* Roles are dictated at the start of an instance. You cannot add roles once the instance is spawned.

* You can however edit the policies that are inside that role, these changes occur immediately.

* If you delete the one and only role on your ec2 instance. You are fucked, you are looking at a terminate and new start.

## CLI Commands for the Dev Exam

* aws ec2 describe-instances
> Instances available to us
* aws ec2 describe-images
> Images available to us to provision instances from
* aws s3 ls
> Lists all s3 buckets.
* start-instances VS run-instances
> aws ec2 start-instances is used to start, stopped instances
> aws ec2 run-instances is used to boot new instances

## Bash Scripting

* Setting up an S3 bucket, just learnt that the s3 bucket namespace is shared, so you have to pick something unique.

* Key pairs are region specific too, they do not track globally, which is a good thing really, but requires management of a host of keys.

* Pretty cool little lab really, speeds up the process even more. So if you were running a web dev firm, you'd be able to provision standard servers with your details and whatnot super quickly.

## Lambda

* Apparently not currently being asessed in the associate exams, but many expect this to change soon.

* Lambda provides a compute service so you can upload code and everything is taken care of for you. No provisioning of servers yourself, no updating, nothing. You just upload and lambda handles the infrastructure.

* Event driven, basically, you set trigger events and lambda reacts to input.

* Enables creation of serveless environments, you now don't have to pay for small severs, you literally pay for interactions with your site.

* Languages supported:
1. Node.js
2. Java
3. Python
4. C#

### Lambda Pricing:

* Nnumber of requests:
> First 1 million requests are free. $0.20 per 1 million requests thereafter.

* Time spent.

# Missed Content. Go back and cover load balancers and SDKs.


## S3

* S3 is Object based, as opposed to block based. Meaning that files can be uploaded but it cannot be used run OS's off.
* Size includes 0bytes to 5TB.
* Unlimited Storage.
* Files (Objects) are stored in Folders (Buckets).
* S3 is a universal namespace, ie: names are unique.
* S3 link format is testable, links look like:
> https://s3-eu-west-1.amazonaws.com/mybucket

* Read after Write consistency for PUTS of new Objects.
> When you create an object, it is instantly readable.

* Eventual Consistency for overwrite PUTS and DELETES.
> Updating and Deleting objects takes much longer to propagate through S3. Not instant.

### S3 Storage Classes/Tiers

* S3 - Durable, Immediately Available, Freq. Access.
* S3 IA - Durable, Immediately Avail., Infreq. Access.
* S3 RRS - Reduced Redundancy Storage, easily reproducible data, thumbnails.
* Glacier - Archival, 3-5 hours for retrieval.

### Core Elements of an S3 Object

* Key - Name
* Value - Data
* VersionID
* Metadata
* Subresources (Access Control Lists, Torrent)

> Keys are lexographical, they store alphabetically.

## CloudFront - CDN

* Edge Location - Location where content is cached. Separate from an AWS Region.
* Origin - Origin of the file that CF will distribute. Whether that is an S3 bucket, EC2, ELB or Route53.
* Distribution - Name of the CDN, contains collection of Edge Locations.

#### Types of Distributions
* Web Distribution - Typically used for websites.
* RTMP - Media Streaming.

* Edge Locations are used for write access as well as read access.
* Objects are cached for the life of the TTL.
* You can clear cached objects intentionally to make way for new ones, but you will be charged.

## AWS Snowball 

* Import/Export styled data migration service.
* Used to import data to and from AWS (S3).
* Briefcase sized small option.
* Literal truck sized large option.
* Data stored in glacier has to be restored to S3 before being uploaded to snowball.

## S3 Transfer Acceleration
* Useful if you have users spread globally.
* Pretty much useless if you don't have global users.

## Cross Origin Resource Sharing (CORS)
*

##Databases

### Types of DBs on AWS.

* RDS - OLTP - Relational Databases Systems
1. SQL
2. MySQL
3. PostgreSQL
4. Oracle
5. Aurora
6. MariaDB
> Apparently don't need to know much about RDS to pass the Dev Exam.

* DynamoDB - NoSQL

* Redshift - OLAP - OnLine Analytics Processing

* ElastiCache - In Memory Caching
1. Memcached
2. Redis 


* DMS - Database Migration Services
> Allows you to move exisiting expensive licensed DBs (Oracle) to AWS and onto free OS DBs like MySQL.


## DynamoDB - Most important section of the course for taking the exam.

* DynamoDb is a NoSQL DB Service. Fully managed and supports both Document and Key-Value data models. Flexible and reliable.

* Stored on SSD.

* Spread across 3 geographically distinct data centres. These are not availability zones however, and should be thought of differently.

* Eventual Consistent Reads (Default):
> Data is PUT into one data centre first, before being copied across to the other two centres within a second.

* Strongly Consistent Reads.
> Data is consistent across all data-centres at all times.

If you can handle the 1 second update time, Eventual is good. If not, Strong is best. I would assume that if your business only operates in one country (is not a global operation) you will be fine with Eventual.

### Elements of DynamoDB

* Tables
* Items (Rows) - Individual Entities (StudentID)
* Attributes (Columns) - Characteristics of Entities (Age of Student, Names)

### Pricing 

* Provisioned Throughput Capacity
1. Write Throughput $0.0065 per hours every 10 units.
2. Read Throughput $0.0065 per hour for every 50 units.

* First 25GB stored per month is free:
1. Storage costs of $0.25 GB per moth thereafter.

## DynamoDB Indexes & Streams

* Two types of Primary Keys:
1. Single Attribute (UniqueID)
> Partition Key (Hash Key) composed of one attribute.
2. Composite ( UniqueID & Date Range)
> Partition Key & Sort Key (Hash & Range) comprised of two atttributes.

* Single Attribiute - Partition Key
> DynamoDB uses the partition key's value as input to an internal hash function. The ouput from the hash function determines the partition ( the physical location in which the data is stored).

> No two items in a table can have the same partition key value.

> Think of users, one user to a partition key.

* Composite - Partition Key & Sort Key
> DynamoDB uses the partition key's value as input to an internal hash function. The ouput from the hash function determines the partition ( the physical location in which the data is stored).

> Two items can have the same partition key, but they must have a different sort key.

> All items with the same partition key are stored together, in sorted order by sort key value.

> Think of users again, one user to one partition key, however when a user makes a forum post, he now generates individual sort key's for each post. When can then search for a user's posts with the partition key.


## Indexes 

1. Local Secondary Index
* Has the SAME partition key, different sort key.
* Can ONLY be created when creating a table. Cannot be removed or modified later.

2. Global Secondary Index
* Has a DIFFERENT partition key, and different sort key.
* Can be created at table creation, or later.

## Streams

* Pretty new, may not yet be in exams.

* Used to capture any kind of modification of the DynamoDB tables. 

## Queries vs Scans

### Queries 
* A query operation finds items in a table using ONLY primary key attribute values. You must provide a partition attribute name and a distinct value to search for.

* You can also optionally provide a sort key attribute name and value, and use a comparison operator to refine the search results. 

### Scans 
* A scan operation examines every item in the table. 

> Try to use queries over scans, as they are more efficient.You don't dump the entire table.

## Calculating R/W Provisioned Throughput

### Unit of Read provisioned throughput
* All reads are rounded up to increments of 4KB.
* Eventually Consistent Reads (default) = 2 reads per second.
* Strongly Consistent Reads = 1 read per second

### Unit of Write provisioned throughput
* All writes are 1kb
* All writes consist of 1 write per second

> EXAMPLE QUESTION = YOU HAVE AN APPLICATION THAT REQUIRES TO READ 10 ITEMS OF 1KB PER SECOND USING EVENTUAL CONSISTENCY. WHAT SHOULD YOU SET THE READ THROUGHPUT TO?

#### Key Figures
* 10 Read Items of 1kb each 
* Eventual Consistency = /2
* Read items have a default value of 4kb. 

#### Equations
* Round all 1kb items to 4kb.
* = 10 read items
* Divide by 2 for eventual consistency. 
* 5 units of read throughput

> EXAMPLE QUESTION = YOU HAVE AN APPLICATION THAT REQUIRES TO READ 10 ITEMS OF 6KB PER SECOND USING EVENTUAL CONSISTENCY. WHAT SHOULD YOU SET THE READ THROUGHPUT TO?

#### Key Figures
* Default read units of 4kb.
* Round all units upto next increment of 4.
* 6kb -> 8kb = 2 read units per item. 
* 10 items x 2 read units per item = 20 
* 20 read units / 2 for eventual consistency.
* 10 units of read throughput required.

> EXAMPLE QUESTION = YOU HAVE AN APPLICATIO  THAT REQUIRES TO READ 5 ITEMS OF 10KB PER SECOND USING EVENTUAL CONSISTENCY. WHAT SHOULD YOU SET THE READ THROUGHPUT TO?

#### Key Figures
* Default read units of 4kb
* Round all units to next increment of 4.
* 10 -> 12 
* 12kb/4kb max = 3 read units per item.
* 5 items * 3 read units per item = 15 read units
* 15 read units / 2 for eventual consistency
* round 7.5 -> 8
* 8 units of read throughput

> EXAMPLE QUESTION = YOU HAVE AN APPLICATION THAT REQUIRES TO READ 5 ITEMS OF 10KB PER SECOND USING STRONG CONSISTENCY. WHAT SHOULD YOU SET THE READ THROUGHPUT TO?

#### Key Figures
* Default read units of 4kb 
* Round all units to next increment of 4. 
* 10 -> 12
* 12kb/4kb max = 3 read units per item. 
* 3 read units * 5 items = 15 read units
* 15 read units / 1 for strong consistency
* 15 units of read throughput 

> EXAMPLE QUESTION = YOU HAVE AN APPLICATION THAT REQUIRES TO WRITE 5 ITEMS, WITH EACH ITEM BEING 10KB IN SIZE PER SECOND. WHAT SHOULD YOU SET THE WRITE THROUGHPUT TO? 

#### Key Figures
* Default write units of 1kb per secpnd
* 5 items @ 10kb in size per second = 50kb per second
* 50kb per second / 1kb per unit = 50 units
* Write throughput of 50 units.

> EXAMPLE QUESTION = YOU HAVE AN APPLICATION THAT REQUIRES TO WRITE 12 ITEMS OF 100KB PER ITEM EACH SECOND. WHAT SHOULD YOU SET THE WRITE THROUGHPUT TO? 

#### Key Figures
* Default write unit size of 1kb per second.
* 12 items @ 100kb = 1200kb per second
* 1200kb per second / 1kb per unit = 1200 units
* Write throughput of 1200 units.

### Point to remember
* Should you hammer your DynamoDB with more data R/W operations than it is provisioned for, you will get a 400 HTTP STATUS CODE - ProvisionedThroughputExceededException
> You exceeded your maximum allowed provisioned throughput for a table or for one or more global secondary indexes.

## DynamoDB with Web Identity Providers
* You can authenticate users uing Web Identity providers (Facebook, Google, Amazon or any other Open-ID Connect-Compatible Identity Provider). 
* This is done using AssumeRoleWithWebIdentity API.
* Need to create a role to use the API.

### Steps taken to Auth with WIP 
1. User authenticates with ID provider (Facebook)
2. They are passed a token by the ID provider.
3. Your code calls AssumeRoleWithWebIdentity API and provides the providers token and specifies the ARN for the IAM Role.
4. App can now access DynamoDB from between 15 minutes to 1 hour (default = 1hr).

## Conditional Updates vs Atomic Counters

* Conditional updates are known as indempotent, that is that they only happen a condition is met. IE: We can create an update that says 'set price at $15 if price is >=$10' 

* Atomic Counters are not indempotent, they aren't intelligent, they just increment or decrement, if a request fails and you're not sure if the request completed, hitting it again, may increment or decrement the item a second time.

> If data has to be 100% accurate, use Conditional Updates. If not, use Atomic Counters.

## Route53 Routing Policies

### Simple Routing
* Default policy when you create a new record set.
* Most common when you have a single resource that performs a given function for a domain.
> One web server that serves content for the http://example.com website

### Weighted Routing
* Allows us to weight routing paths in % terms. 
* Perfect for A/B testing.
* Can route across and within AZ's.
> 100 requests are made, we have a weighted policy that pushs 30% to AP-Southeast-1 (or a load balancer) and 70% to AP-Southeast-2 (or a load balancer).

### Latency Routing 
* Allows us to route traffic based on lowest possible latency for end user. 
* Effectively location based, it will route users to the best AZ for them.
> You have users globally and wish to point them to the best possible region to provide them the lowest latency.

### Failover Routing 
* Allows us to create healthchecks for a server, and should a healthcheck fail, traffic will be redirected to a passive server.
* Referred to as active/passive routing. 
> Healthcheck fails on active server, all traffic is then routed through to the passive server.

### Geolocation Routing
* Could be viewed as very similar to Latency Routing.
* In this instance however, routing is based on user's location and the content they should be seeing. 
> User from UK and User from Aus visit their favorite website www.buyexamples.com and are redirected to their relevant TLD's. .uk and .au this happens because Aus customers may have a different product selection or shipping options.

