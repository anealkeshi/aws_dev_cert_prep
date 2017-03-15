# AWS Certified Developer Prep 

> This repo will be used as I prepare for my AWS Certified Developer certification. I will be committing all notes and code I write over the course of my preparation. 

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

## EC2 Exam Tips

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





