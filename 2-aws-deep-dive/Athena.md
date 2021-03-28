# Athena

Servierles sservice to perform analytics directly against s3 files
uses sql language to query the files
has a jdbc/odbc driver
charged per query and amount of data scanned
supports csv, json, orc, avro and parquet(built on presto)
User cases: business intelligence / analytics / reporting, analyze and query VPC FLow logs, 
ElB logs, cloudtrail trails

use case: if you want to run analytics directly on the s3 buckets use athena