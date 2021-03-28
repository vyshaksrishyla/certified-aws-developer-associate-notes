# Advanced S3

## S3 MFA Delete

* To use MFA Delete enable versioning on the bucket  
* You will need MFA to 
* Permanently deltea n object version
* suspend version on the bucket

* you wont need mfa for
* enable versioning
* listing deleted versions

* only the buket owneer (root account) can enable disable mfa delte
* mfa dlte currently can onlby be enabled using the cli.

## S3 Access Logs

* For audit purpose, you may want to log all access to s3 bucket.
* Log all account, authorized or denied
* Data can be anlyed by data analysis tools or Amazon Athena.
Warning  
* Do not set your logging bucket to be the monitored bucket  
* it will create a logging loop, and your bucket will grow in size exponentially  
* Always seperate Application bucket and logging bucket.  

## S3 Replication (CRR & SRR)

* Must enable versioning in source and destination
* Cross Region Replicaion
* Same Region Replication
* Buckets can be in different accounts
* Copy is asynchrnous
* Must give proper iam permissions to s3
* Crr - use cases: compliance, lower latency acces, replication across acounts
* srsr- use case" log aggregation, live replication between production and test accounts

* S3 Replication - NOts
    * After acitvation, only new objects are replciation (not retroactive)

* For Delete operations
  * if you delete without a version id, it addsa a delete marker, not replications
  * if you delete wieth av ersion id , it deltes in source, not replication

* THere is no chaining of replciation
  * if bucket i has replication into bucket 2, which has replication into bucket 3
  * then objhects created in bucket i are not replication to bucekt 3

## S3 pre-signed URLs

Can generate presigned urls using sdk or cli
    for downloads easy can use the cli
    for uploads harder, must use the sdk
Valid for a default of 3600 seconds, can change timeout with --expires-in [TIME_BY_SECONDS] argument  
Users given a pre-singed URL inherit the permissions of the person who generated the URL for GET.PUT

Examples:
ALllow only logged in users to download a peremioum video on your s3 bucket
allow an ever changing list of users to downloadfiles by genration urls dynamically
allow temporatily a user to uplaod a file to a precise location in our bucket

## S3 Storage Classes

* Amazon s3 standard - Genreal purpose
AMson s3 standard -infrequent access (IUA)
S3 one one -infrequent access
amason s3 intelligent tiering
amason glaier
glacier deep archive
Reduced Redundancy storage (depricate)

## S3 general purpose

* High durability (99.999999999%) of objects across multiple AZ
* If you store 10,000,000 objects with amzon s3, you can on average expect to incur a loss of a single object once every 10,000 years
* 99.99% availability over a given year
* SUstain 2 concurrent facility failures
Use cases: big data anaytics, mobile & gamin applicitons, content distirbution.

## S3 Standard - Infrequent Access (IA)

* Suitable for data that is less frequently accessed but requires rapid access when needed
* High durability (99.999999999) ob objects across multiple AZs
* 99.9 availability
low coset compared to amazon s3 standard
sustain 2 concurrent facility failures
User cases: backups, disaster recoverly

## s3 one zone - infrequence access (ISA)

* Same as IA but data is stored in a single az
high durability of objects in a single a, data lost when az is destroyed
99.5 availability
low latency and high througput perfomence
supports ssl for daga at trasit and ecntyption at rest
low cost compate dot is ayb 20

Use cases:storing secondary backup coopes of on premise datga or storing data you can recreate

## se intelligent tiering

same low latency and hight thorugput performance of s3 standard
small monthinly mointoinr and autio tiering fee
automatically moves objects between two acess tiers based on changing access patterns
designed for duralbitliyt of objecgs accrross multiple abvailabity zones
resilient against ebents that impact an entire a
design for 99,9 abailblity over a given year

AMazon glaier

archive glacier
low cost object sotrage meant for athivging. backup
data is retained for the longer term
alternative to on premise magnetic tape storegage
averange annual durablity is 00.9999
cost per stoage per month 0.004 /gb + retirevela cost
each item in glavier is called archive
archeive are stored in vaults


AMazon glacier & glaicer deep archive

amsong glacier - 3 retrievel optins
expediate (1 to 5 mintus)
standard (3 t0 5 hours)
bulk (5 to 12 hours)
minimum storage duration of 90 days

AMazon glacier deep archive - for long term stoage -cheaper:
standard - 12 hours
bulk - 48 hours
minim storage duration of 180 days

## S3 lifecycle rules

You can transition objects between storage classes.
For infrequeently access object, mobe them to standard_IA
for archive objects you dont need in ral time, glacier or deep archive

Transtion actions: it defines when objects are transtioned to another storage class
    move objects to standard IA class 60 days after creation
     move to glacier for rchiving after 6 months

Expriation actions: configure objects to expire ( delte ) after sometime
    Access log files can be set to delte after a 365 days
    can be used to delte old versions of files (if versioning is enabled)
    can be used to delte incomplete multi part uploads

Rules can be created for a certain prefix (ex - s3:..bucket/mp3)
Rules can be created for certain object tags (ex - Department: Finance)

## S3 Performacne

AMazon s3 atuomatincally scales to high request rates, laatency 100-200 ms
your application can achieve at least 3500 PUT/COPY/POST/DELTE and 5500 GET/HEAD requests per second per prefix in a bucket.
There are no lite to the number of prefixes in a bucket
Example (ojbect path => prefix)
    bucekt/folder1/sub1/file => /foler1/sub1/

## KMS Limitation
You may be impacted by kms limits
when you upload it calls generate datakey kms api
when you download, it calls the decrypt kms api
count towards the kms quota per second
5500,10000,30000 req/s based on region

As of today, you cannot request a quota increase for kms

## S3 performacne
Multi part upload
    recommended for file > 100 MB,
    must use for files > 5 GB
    Can help parallelie uploads (speed up transfers)

S3 Transfer Acceleration (upload only)
Increase transfer speed by transferring file to an aws edfe location which wil forward the data to s4 bucket in the target region.
compatible with multi part upload

S3 performane - s3 byte range fetches
parallelize gets by requesting specific byte ranges
better resilience in case of failures
can be ysed to speed up donwloads

Can be used to retrieve only partial data (for example the head of a file)

## S3 Select & Glacier Select
* Retrieve less data using SQL by performaing server side filetering
* can fileter by rows and columns 
* less netwrok transfer, less cpu cost client -side

## S3 event notifications

react by
object name filteing possible (*.jpg)
User case: thumbnails of images uploaded to s3
targets: 
SNS, SQS, Lambda function.

## S3 Object Lock & Glacier Vault Lock

* S3 Object Lock
    * Adopt a WORM ( Write Once Read Many ) model
    * block object version deleteion
* Glacier Valult lock
    * prevents future edits to the file
    * helpful for compliance and data retention requirements

