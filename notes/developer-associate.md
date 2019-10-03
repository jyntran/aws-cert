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
- to support regulatory requirements and where multi-tenant virtualization is not an option
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

SSD
- General Purpose SSD (GP2)
  - up to 10,000 IOPS
- Provisioned IOPS SSD (IO1)
  - relational or NoSQL databases
  - 10,000 - 20,000 IOPS

Magnetic
- Throughput Optimized HDD (ST1)
  - big data
  - data warehouses
  - log processing
  - cannot be a boot volume
- Cold HDD (SC1)
  - infrequently accessed
  - cannot be a boot volume
- Magnetic (Standard)
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

Application load balancers
- HTTP/HTTPS
- Layer 7, application (request level)
- intelligent, advanced routing and requests

Network load balancers
- Layer 4, connection
- performance, low latencies

Classic load balancers
- Layer 7, using X-Forwarded-For and sticky sessions
- Layer 4
- ELB usually refers to classic

504 error
- gateway timed out, app not responding within idle timeout period
- troubleshoot app: is it the web server or database server?

X-Forwarded-For
- header in request
- contains the IPv4 address of end user

## Route53

Amazon's DNS service

Map domain names to
- EC2 instances
- load balancers
- S3 buckets

Able to register/purchase domains

## CLI

*Command Line*

Least privilege
- always give users minimum access

Create groups
- easier to manage many users using policy documents

Secret access key
- see it once, keep it safe
- do not use just one, create one pair for each developer
- easier to deactivate if person leaves or if compromised
- prefer roles over access keys whenever possible

Already installed on EC2

Can use on own PC

## RDS

*Relational Database Service*

Relational Databases
- Table
- Row
- Fields

Non Relational Databases
- Collection
- Document
- Key Value Pairs

No SQL Types
- DynamoDB

Data Warehousing
- for business intelligence
- pull in very large and complex data sets
- usually used by management to query data (current performance, targets, etc.)

Online transation processing (OLTP)
- simple, quick, frequent
- e.g. online shopping order
- relational databases
  - Microsoft SQL Server
  - Oracle
  - MySQL Server
  - PostgreSQL
  - Amazon Aurora
  - MariaDB

Online analytics processing (OLAP)
- complex, multiple calculations
- e.g. net profit for (Europe, Middle East, Africa) EMEA and Pacific for Digital Radio Product
- uses different type of architecture from database perspective and infrastructure layer
- Redshift

Backup types
- Automated Backups
  - default setting
  - retention period 1-35 days
  - takes full daily snapshot and store transaction logs
  - during recovery, AWS chooses most recent backup and apply transactions
  - point-in-time recovery down to a second
  - free S3 space to store backup
  - backup window (start time and duration) can be edited
- Database Snapshots
  - manual, user initiated
  - stored even after deleting original RDS instance
- when restoring, it will be a new RDS instance with a new DNS endpoint

Encryption
- at rest supported by MySQL, Oracle, SQL Server, PostgreSQL, MariaDB and Aurora
- done using KMS
- data, automated backups, read replicas, and snapshots are all encrypted

Multi-AZ
- AZ: availability zone
- automatic failover to a different AZ
- when prod database is written to, so is the standby database
- is for disaster recovery only

Read Replicas
- primarily for read-heavy database workloads
- can have 5 read replicas per production database by default
- can have EC2 instances read from read replicas to take load off prod database
- can have read replicas off read replicas (watch for latency)
- each have its own DNS endpoint
- can be in different AZ or regions (for MySQL and MariaDB), can have Multi-AZ
- using asynchronous replication
- not yet available for Oracle or SQL Server
- must have automatic backups enabled
- can be promoted to be their own databases (breaks replication)
- is for scaling, performance improvement only

## Elasticache

Easily deploy, operate, and scale in-memory cache in the cloud for low latency access
- result of I/O-intensive database queries or computationally-intensive calculations
- e.g. cache top ten most sold items

Two open-source caching engines
- Memcached
  - service can be integrated into Elasticache
  - treated as a pure caching solution
  - as simple as possible
  - nodes as a pool that can grow and shrink horizontally, scale out
  - auto node replacement and auto discovery
- Redis
  - key-value store
  - supports data structures such as hashes, sorted sets, and lists, e.g. leaderboards
  - supports Master/Slave replication and Multi-AZ with failover
  - data persistence is important
  - Pub/Sub capabilities are needed
  - treats as relational database

When to use
- Elasticache: load is read heavy, data not prone to frequent changing
- Redshift: OLAP transactions are constantly ran by management