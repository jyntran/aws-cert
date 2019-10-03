# AWS Developer Associate Notes

## IAM

*Identity Access Management*

Consists of
- Users
- Groups
- Roles
- Policy documents

Universal - does not apply to regions

New users have no permissions when created

Password
- used to log into the AWS Management console

Access Key ID and Secret Access Key
- can be generated for a user
- viewable and downloadable only once
- used for CLI and API

Always set up Multifactor Authentication (MFA) on the root account

Password rotation policies can be created and customized

## EC2

*Elastic Compute Cloud*

On Demand
- fixed rate by second (hour)
- low cost without long-time commitment

Reserved
- up front payments
- contract for 1 or 3 years
- types
  - standard
  - convertible
  - scheduled

Spot
- flexible start and end times
- bid for the price you want
- if terminated by EC2, not charged for partial hour
- if terminated by you, charged for complete hour

Dedicated hosts
- to support regulatory requirements and for those with no support for multi-tenant virtualization
- can be purchased on demand or as a reservation

Instance types
- F - Field Programmable Gate Array (**FPGA**)
- I - High Speed Storage (**IOPS**)
- G - **Graphics** Intensive
- H - **High** Disk Throughput
- T - Lowest Cost, General Purpose (**T2 micro**)
- D - **Dense** Storage
- R - Memory Optimized (**RAM**)
- M - General Purpose **Main**
- C - **Compute** Optimized
- P - Graphics/General Purpose GPU (**Pics**)
- X - Memory Optimized (**Xtreme** memory)

EC2 with Roles
- more secure than access keys
- easier to manage
- controlled by policies, takes effect immediately
- can attach/detach without stopping or terminating running instances

## EBS

*Elastic Block Store*

Storage volumes attached to EC2 instances
- must be in the same availability zone

General Purpose SSD (GP2)
- up to 10,000 IOPS

Provisioned IOPS SSD (IO1)
- relational or NoSQL databases
- 10,000 - 20,000 IOPS

Throughput Optimized HDD (ST1)
- big data
- data warehouses
- log processing
- cannot be a boot volume

Cold HDD (SC1)
- infrequently accessed
- cannot be a boot volume

Magnetic (Standard)
- infrequently accessed
- lowest cost

Encryption
- if from encrypted snapshot, encrypted
- if from unencrypted snapshot, not encrypted
- if not from snapshot, have choice
- can encrypt additional volumes using console, CLI, or API

Encrypting root volume
- at OS level (e.g. on Windows, using Bitlocker)
- with snapshots
  - take snapshot
  - create copy of snapshot and choose encryption
  - create image (AMI) from snapshot
  - deploy instance from image

## ELB

*Elastic Load Balancer*

Application load balancer
- HTTP/HTTPS
- Layer 7, application (request level)
- intelligent, advanced routing and requests

Network load balancer
- Layer 4, connection
- performance, low latencies

Classic load balancer
- Layer 7, using X-Forwarded and sticky sessions
- Layer 4

## Route53

Amazon's DNS service

Map domain names to
- EC2 instances
- load balancers
- S3 buckets

Able to register/purchase domains

## CLI

*Command Line*

Already installed on EC2

Can use on own PC

## RDS

*Relational Database Service*

Relational Databases
- Table
- Row
- Fields

Types that can be provisioned
- Microsoft SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- Amazon Aurora
- MariaDB

Non Relational Databases
- Collection
- Document
- Key Value Pairs

- Types
- DynamoDB

Data Warehousing
- for business intelligence
- pull in very large and complex data sets
- usually used by management to query data (current performance, targets, etc.)

Online transation processing (OLTP)
- simple, quick, frequent
- e.g. online shopping order
- relational databases

Online analytics processing (OLAP)
- complex, multiple calculations
- e.g. net profit for (Europe, Middle East, Africa) EMEA and Pacific for Digital Radio Product
- uses different type of architecture from database perspective and infrastructure layer
- Redshift

Elasticache
- easily deploy, operate, and scale in-memory cache in the cloud
- e.g. cache top ten most sold items
- two open-source caching engines
  - Memcached
  - Redis