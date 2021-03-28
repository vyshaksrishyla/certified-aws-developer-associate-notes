# CLI: Command Line Interface

Add user credentials locally using this command:
- $ `aws configure`

If you are using multiple AWS accounts, you can add custom profiles with seperate credentials using this command:
- $ `aws configure --profile {my-other-aws-account}`
- if you you'd like to execute commands on a specific profile:
  - example: `aws s3 ls --profile {my-other-aws-account}`
- if you don't specify the aws profile, the commands will be executed to your **default** profile

#### AWS CLI on EC2
* IAM roles can be attached to EC2 instances
* IAM roles can come with a policy authorizing exactly what the EC2 instance should be able to do. This is the best practice.
* EC2 Instances can then use these profiles automatically without any additional configurations

#### CLI STS Decode Errors
- When you run API calls and they fail, you can get a long, encoded error message code 
- This error can be decoded using STS 
- run the command: `aws sts decode-authorization-message --encoded-message {encoded_message_code}`
- your IAM user must have the correct permissions to use this command by adding the `STS` service to your policy


### MISCELLAENOUS
--dry-run command

Instane metadata:  
http://169.254.169.254/latest/metadata

## AWS Limits ( Quotas )  
API Rate Limits  
DescribeInstances API for EC2 has a limit of 100 calls per seconds  
GetObject on S3 has a limit of 5500 GET per second per prefix  
For Intermittent Errors: implement Exponential Backoff  
For Consistent Errors: request an API throttling limit increse  

## SErvice Quotas (Service Limits)  
Running On-Demand Standard Instances: I 152 vCPU
You can request a service limit increase by opening a ticket  
You can requests a service quota increase by using the service quotas api  


## AWS CLI credentials provider chain  
* Priorities based on  
    * command line options
    * Environment variables
    * CLI credentials file
    * CLI configuration file
    * container credentials
    * Instance profile credentials

## Signing AWS API requests
Signature v$ (SigV4) - request to AWS are signed using credentials
HTTP Header option  
Query String option (ex: s3 pre-signed URLs)