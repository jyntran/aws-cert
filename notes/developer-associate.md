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
- sits between application and database

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

Lazy Loading
- loads from cache
- only requested data is cached, and avoids filling cache with useless data
- node failures are not fatal
- if cache miss, query to database, and write to cache
- stale data can remain in cache
  - can set TTL to avoid this

Write-through
- data in the cache are never stake
- user are generally more tolerant of additional latency when updating data than when retrieving it
- write penalty, every write involves writing to cache and to DB
- if node fails, data is missing until added or updated in the database
  - can be mitigated by implementing both LL and WT
- wasted resources if most of the data is never read

## S3

*Simple Storage Service*

Object-based storage, not block storage
- spread across multiple devices and facilities
- high availability and disaster recovery

Files
- 0 bytes to 5 TB
- unlimited storage
- stored in Buckets (similar to folders)
- universal namespace, names must be unique globally
- e.g. https://s3-eu-west-1.amazonaws.com/UNIQUENAME
- HTTP 200 when upload successful from CLI or API

Data consistency
- Read after Write: file is available to read immediately after PUT
- Eventual Consistency: overwrite PUTs and DELETEs can take some time

Object-based, key-value store
- objects consist of
  - key: name of object
  - value: the data, sequence of bytes
  - version ID: important for versioning
  - metadata
  - subresources: bucket-specific configuratuon
    - bucket policies, access controls lists
    - cross origin resource sharing (CORS)
    - transfer acceleration

Availability
- designed for 99.99 availability for S3 platform
- Amazon guarantees 99.9% availability
- Amazon guarantees 99.999999999 durability for S3 information (11 x 9's)
  - how much data expected to keep within a year
  - stored redundantly across multiple devices
  - designed to sustain a loss of 2 facilities concurrently
- always have backup of data
- tiered storage available
- lifecycle management
- versioning
- encryption

Storage Tiers/Classes
- S3
- S3 - IA (Infrequently Accessed)
  - 99.9% availability
  - requires rapid access when needed
  - lower fee, but charged retrieval fee
- S3 - One Zone IA
  - stored in a single AZ only
  - only 99.5% availability
  - cost is 20% less than regular S3 IA
- Glacier
  - 99.99% availability after you restore objects
  - for archival only
  - very cheap
  - for infrequently accessed data
  - can take 3-5 hours to retrieve
- Reduced Redundancy Storage
  - 99.99% durability
  - 99.99% availability
  - used for data that can be recreated if lost, e.g. thumbnails
  - phasing out? starting to not be available in some regions, and more expensive than S3

Intelligent Tiering
- unknown or unpredictable access patterns
- 2 tiers
  - frequent access
  - infrequent access
- automatically moves fate to more cost-effective tier-based on how frequently objects are accessed
- 99.999999999% durability
- 99.9% availability
- optimizes costs
- no fees for accessing data
- small monthly fee for monitoring/automation $0.0025 per 1,000 objects

Charges
- charged for storage per GB, requests (get, put, copy, etc.), storage management pricing (inventory, analytics, and object tags)
- data management pricing
  - data transferred out of S3
- transfer acceleration
  - use of CloudFront to optimize transfers

Security
- all newly created buckets are private
- can be configured to create access logs, which can be written to another bucket

S3 Website Hosting
- always use website URL (https://BUCKETNAME.s3-website-REGION.amazonaws.com) instead of S3 URL (https://s3-REGION-.amazonaws.com/BUCKETNAME)

ACL
- access control list
- per object basis

Properties
- Versioning
  - if object is deleted, can be restored to a previous version
- Server access logging
  - for auditing
- Tags
  - to track project costs
- Object-level logging
  - uses CloudTrail for an additional cost
- encryption
  - AES-256 - default Amazon S3 encryption
  - KMS
- Cloudwatch metric logging

Metadata
- not modifiable after object is created

Policies
- can be written with JSON or by the Policy Generator

In Transit
- SSL/TLS, using HTTPS to upload

At Rest
- Server Side Encryption (SSE)
  - S3 Managed Keys - SSE-S3
  - AWS KMS Managed Keys - SSE-KMS
    - includes audit trail
    - can use default key unique to you or create a custom one
  - Server Side Encryption with Customer Provided Keys - SSE-C 
    - key rotation
- Client Side Encryption
  - encrypt files with client before upload

Enforcing encryption
- upload is a PUT request
- host
- date/timestamp of upload
- authorization string
- content-type
- content-length
- user-defined metadata
- expect
  - AWS can reject upload based on header

Encryption header
- `x-amz-server-side-encryption: AES256 | ams:kms`
- encrypt at time of upload
- can deny all PUT requests that don't include this parameter in headers from bucket policy
  - condition: `"StringNotEquals"`
  - returns 403 forbidden

CORS Configuration
- Static website hosting
  - bucket can be used to host website
  - grant public access
- for CORS, files across multiple buckets

Performance optimization
- if receiving >100 PUT/LIST/DELETE requests or >300 GET requests
- GET-intensive workloads
  - use Cloudfront CDN
- Mixed request type workloads (GET, PUT, DELETE)
  - key names can impact
  - sequential key prefixes increase likelihood to be on same partition
  - to optimize, introduce randomness into prefix of key name

## Cloudfront

CDN
- content delivery network
- system of distributed servers (network) delivers content based on geographical location

Edge Location
- where content is cached and can be written
- separate to a region/AZ
- cached for life of (Time to Live) TTL in seconds
- can clear caches manually, but will be charged

Origin
- where all files that the CDN will distribute
- can be S3 bucket, EC2, ELB, Route53

Distribution
- name which consistes of collection of Edge Locations
- two types:
  - web distribution
    - websites over HTTP/HTTPS
  - RTMP distribution
    - Adobe Real Time Messaging Protocol, for media streaming, Flash media content
- optimized to work with AWS servers as well as non-AWS servers

Transfer Acceleration
- takes advantages of Cloudfront's edge locations

Restrict Viewer Access
- can restrict to users who use signed URLs or signed cookies

Web Application Firewall (WAF)
- at application layer

Alternate Domain Names
- can add

Geo-Restrictions
- whitelist/blacklist certain countries

## Serverless

No worrying about servers, database administration

Independent, 1 event = function

Can trigger other functions, 1 event can = x functions

Continuous scaling
- scales out (not up) automatically

Lambda
- is serverless
- takes care of provisioning and managing services to run code
- can run code in response to events
- as computer service in response to HTTP requests using API calls using SDKs

Languages
- NodeJS
- Python
- Java
- C#
- Go

Number of requests
- first 1 million requests are free. $0.20 per 1 million requests thereafter

Duration
- calculated from the time code begins executing until it returns or terminates

Echo and Alexa
- voice to text to Lambda

Serverless services
- DynamoDB
- S3
- Lambda

Architectures can get complicated, AWS X-ray can debug Lambda

Can be global, can back up S3 buckets to other S3 buckets

## API Gateway

API
- waiter in restaurant analogy

Types of APIs
- REST (Representational State Transfer)
  - JSON
- SOAP (Simple Object Access Protocol)
  - XML

Amazon API Gateway
- full managed service make it easy for developers to manage APIs
- exposes HTTPS endpoints to define a RESTful API
- serverless-ly connect to services like Lambda and DynamoDB
- send each API endpoint to a different target
- scales effortlessly
- track and control usage by API key
- can throttle requests to prevent attacks (e.g. DDoS)
- connect to CloudWatch to log results
- maintain multiple versions of API

Configure
- define API (container)
- define resources and nested resources (URL paths)
- for each resource
  - select supported HTTP v=methods (verbs)
  - set security
  - choose target (service)
  - set request and response transformations
- deploy API to a Stage
  - use API gateway domain by default
  - can use custom domain
  - supports AWS Certificate Manager (free SSL/TLS certs)

API caching
- cache endpoint response with TTL

Same Origin Policy
- web browser permits scripts only if both web pages have the same origin
- to prevent Cross Site Scripting (XSS)

Cross-Origin Resource Sharing (CORS)
- a more relaxed policy
- restricted resources from another domain
- enforced by the client

HTTP Options call
- server returns response: These other domains are approved to GET this URL

Error
- need to enable CORS on API Gateway

## Lambda

Versioning
- each has a unique ARN
- immutable
- only $LATEST can be updated

Qualified/Unqualified ARNs
- qualified: `helloworld:$LATEST` or `helloworld:ALIAS`
- unqualified: `helloworld` (no version)

Alias
- can create a PROD alias, point to version 1, then change to version 2 when ready

Shift traffic
- based on weights (%)
- cannot be done on $LATEST version, instead create an alias to $LATEST

## Alexa

Echo (hardware) and Alexa (software)

Amazon Polly
- text-to-speech service
- can synthesize text to an .mp3 in S3

SSML (synthesis speech markup language)
- `<speak>`
- `<audio src="s3_sound.mp3">`

## Step Functions

- allow to visualize and test serverless applications
- automatically triggers and tracks each step
- retries when there are errors
- logs state of each step
- ensures application runs in order
- under Application Integration
- uses JSON-based Amazon States Language (ASL)

## X-Ray

- collects data about requests
- traces requests; view, filter, and gain insights into that data
- X-Ray SDK -> X-Ray daemon -> X-Ray API -> X-Ray Console
- integrates with
  - ELB
  - Lambda
  - API Gateway
  - EC2
  - Elastic Beanstalk
- languages, same list as supported by Lambda
- interceptors to add to code to trace incoming HTTP requests
- client handlers to instrument AWS SDK clients to call other services

## Advanced API Gateway

Import APIs
- Supports Swagger 2.0 API definition files

API Throttling
- by default, limits steady-rate request rate to 10,000 requests per second (rps) or 5,000 concurrent requests
- if over, receive a 429 Too Many Request error

SOAP Configuration
- as a SOAP web service passthrough

## DynamoDB
- low latency NoSQL database service
- consists of tables, items. attributes
- serverless
- stored on SSD storage
- spread across 3 geographically distinct data centres
- supports document and key-value data models
- documents can be written in JSON, HTML, or XML

Consistency Models
- eventual consistent reads (default)
  - consistency is reached within a second
- strongly consistent reads
  - any writes are reflected in the next read
  - returns a result that reflects all responses

Primary Keys
- Partition Key - unique attribute (e.g. user ID)
  - value is input to an internal hash function which determines the partition or physical location of on which the data is stored
- Composite Key (Partition Key + Sort Key)
  - 2 items may have the same Partition Key, but must have a unique Sort Key

Access Control
- IAM Users
- Roles for temporary access keys, e.g. for EC2 instances to have access to DynamoDB
- Condition to restrict access, e.g. where Partition Key matches their User ID (`dynamodb:LeadingKeys`)

Indexes
- speeds up queries on specific data columns
- Local Secondary Index
  - only created on table creation
  - cannot be added, removed, modified later
  - same Partition Key as original table, but different Sort Key
  - e.g. Partition Key: User ID, Sort Key: account creation date
- Global Secondary Index
  - can be created or added later
  - different Partition Key and Sort Key
  - e.g. Partition Key: email address, Sort Key: last login date

Query
- operation that finds items based on Primary Key attribute and a distinct value to search for
- optional, use a Sort Key to refine
- can use `ProjectionExpression` parameter
  - e.g. if you only want to see the email address rather than all attributes
- results. always sorted by Sort Key in ascending order/ASCII character code values
- reverse by setting `ScanIndexForward` parameter to `false` (queries only)
- by default, all queries are eventual consistent

Scan
- operation examines every item in table
- by default returns all data attributes

Differences
- queries are more efficient than scans
- scan dumps all results then filters
- scan takes longer, can use up provisioned throughput in a single operation
- can set page size to return less which uses fewer read operations
- isolate scan operations to specific tables
- by default, scan returns 1MB increments, scans one partition at a time
- can configure to use parallel scans, best to avoid if table or index is already under heavy use
- if possible, design tables in a way that you can use Query, Get, or BatchGetItem APIs

Provisioned Throughput
- measured in Capacity Units (CU)
- 1 Write CU = 1KB Write per second
- e.g. 600 items per minute, each item consists of 5KB
  - 1 WCU = 1KB of data. 600 /60 = 10 writes per second x 5KB = 50KB written per second, therefore 50 WCU
- 1 Read CU = 1 x 4KB SCR per second OR 2 x 4KB ECR per second (default)
  - i.e. ECR is double the throughput of SCR
- e.g. read 25 items of 13KB per second, eventually consistent
  - each read CU represents 1 x 4KB SCR. 13KB/4KB = 3.25, rounded up is 4, multiply by 25 = 100 SCR/2 50 ECR
- if app reads or writes larger items, will consume more CU and will cost more

On-Demand Capacity
- charges apply for: reading, writing, and storing data
- don't need to specify read/write capacity requirements
- instantly scales, great for unpredictable workloads
- pay for only what you use (pay per request)

DynamoDB Accelerator (DAX)
- fully-managed, clustered in-memory cache
- up to 10x read performance improvement
- ideal for read-heavy and bursty workloads
  - e.g. auction, gaming, retail sites during Black Friday
- write-through caching service
  - allows to point DynamoDB API calls at the DAX cluster (query DAX before DynamoDB)
- if cache miss, DAX performs Eventually Consistent GetItem operation against DynamoDB
- may be able to reduce provisioned capacity, save money
- not suitable for
  - Strongly Consistent read-heavy apps
  - apps not needing low latency
  - write-intensive apps

Transactions
- ACID (Atomic, Consistent, Isolated, Durable)
- read or write multiple items across multiple tables as an all or nothing operation
- check for pre-requisite condition before writing
- good for mission-critical situations, e.g. financial transactions

TTL
- defines expiration time for data
- automatically marked for deletion and removed when current time is greater than TTL
- great for removing
  - session data
  - event logs
  - temporary data
- expressed as epoch timestamp
- expiration set for 2 hours after the session began
- can filter out expired items from queries and scans

Streams
- time-ordered sequence of item level modifications (insert, update, delete)
- logs are encrypted at rest and stored for 24 hours until deleted
- accessed using dedicated endpoint
- by default Primary Key is recorded, minimum amount
- Before and After images can be captured
- applications can take actions based on contents
- can be an event source for Lambda, is polled

Provisioned Throughput Exceeded Exception
- `ProvisionedThroughputExceededException`
  - request rate is too high for read/write capacity provisioned on table
- SDK automatically retries until successful
- if not using SDK
  - reduce request frequency
  - use Exponential Backoff

Exponential Backoff
- progressively longer waits between consecutive retries to improve flow control
  - e.g. 50ms, 100ms, 200ms, ...
- if after 1 minute still doesn't work, request size may be exceeding the throughput for read/write capacity
- featured in many SDKs of services, e.g. S3 buckets, CloudFormation, SES, etc.

## KMS

*Key Management System*

- create and control encryption keys used to encrypt data
- multi-tenant

Customer Master Key (CMK)
- consists of
  - alias
  - creation date
  - description
  - key state
  - key material (either customer provided or AWS provided)
- can NEVER be exported

Setup
- Administrative permissions
  - users/roles who can administer but not use the key through KMS API
- Usage permissions
  - users/roles that can use the key to encrypt/decrypt data

API Calls
- `aws kms encrypt`
- `aws kms decrypt`
- `aws kms re-encrypt`
  - on server side, decrypts then re-encrypts using a new CMK
  - destroys plaintext immediately to avoid exposing it to the client side
- `aws kms enable-key-rotation`

Envelope Encryption
- Master Key encrypts Envelope Key (Data Key) encrypts Data
- Encrypted Data Key decrypted by Master Key which is then used to decrypt Data

Deletion
- must be disabled first
- must be scheduled for deletion (minimum of 1 week wait)

## SQS

*Simple Queue Service*

- distributed message queuing system
- access message queue that can be used to store messages while waiting for a computer to process them
- can decouple components of an app so they run independently
- messages can contain up to 256KB of text in any format
- can be in the queue for 1 minute - 14 days
- default retention period is 4 days
- messages can be retrieved programmatically using SQS API
- queue acts as a buffer between producer and consumer
- pull-based (poll), not push-based

Queue Types
- Standard Queues (default)
- nearly unlimited number of transactions
  - guarantee message is delivered at least once
  - may be out of order
- FIFO Queues (first in first out)
  - guaranteed to be in order
  - limited to 300 transactions per second (TPS)

Visibility Timeout
- amount of time that message is invisible in the SQS queue after a reader picks up that message
- if not processed in time, becomes visible again and another reader picks it up
- can result in message being delivered twice
- default visibility timeout: 30 seconds
- increase it if your task takes >30 seconds
- maximum is 12 hours

Short Polling
- returned immediately even if no messages are in the queue

Long Polling
- polls the queue periodically and only returns a response when a message is in the queue or timeout is reached

## SNS

*Simple Notification Service*

- notification management to different subscribers
- push notifications to Apple, Google, Fire OS, Windows devices, Android in China with Baidu Cloud Push
- deliver by SMS or email to SQS or HTTP endpoint
- can also trigger Lambda functions
- can group multiple recipients using topics, an access point for identical copies of the same notification
- messages are stored redundantly across AZs
- instantaneous, push-based delivery (no polling)
- simple APIs and easy integration with applications
- flexible delivery over multiple transport protocols
- pay-as-you-go-model no up-front cost
- follows publish-subscribe (pub-sub) paradigm

## SES

*Simple Email Service*

- uses
  - marketing emails, newsletters, advertisements
  - purchase confirmations, shipping notifications, order status updates
- pay-as-you-go-model
- can be delivered automatically to an S3 bucket
- can trigger Lambda functions and SNS notifications
- only need to know email address

## Kinesis

- platform to send streaming data
- load and analyze data

Kinesis Streams
- shards
  - multiple shards in stream
- streaming data from producers
- stored for 24 hours, can be configured to be 7 days in shards
- sent to consumers
  - other AWS services

Kinesis Firehose
- producers
- data analysis in Lambda is optional
- sent to consumers
  - S3 then copied to Redshift
  - or to Elasticsearch cluster

Kinesis Analytics
- analysis of shards or Firehose
- uses SQL type of query languages
- no worry about consumers