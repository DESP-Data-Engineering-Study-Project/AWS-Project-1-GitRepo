# Create Data Lake with Amazon S3, Lake Formation and Glue
In this workshop, you will keep two data sets sales and customers in Amazon S3. AWS Glue is used to catalog the data. You then use AWS Lake Formation to provide specific permission for the salesuser and customersuser users for the sales and customers tables. The users will use Amazon Athena to query the data as per the permission defined.

---
 Sections
- [Prerequisites](#Prerequisites)
- [Limitations](#Limitations)
- [Product Versions](#Product-Versions)
- [Architecture](#Architecture)
- [High level work flow](#High-level-work-flow)
- [Repository Structure](#Repository-Structure)
- [Deploy](#Deploy)
- [Sample Workflow Execution](#Sample-Workflow-Execution-and-Notification)

## Prerequisites 

* An active AWS account with programmatic access
* AWS CLI with AWS account configuration, so that you can create AWS resources by deploying CloudFormation  stack
* Amazon S3 bucket 
* CSV dataset with correct schema ( attached is a sample csv file with correct schema and data type)
* Web browser
* AWS Glue console access
* AWS Step Functions console access


## Limitations
AWS Step Functions:

- Execution History: The maximum limit for keeping execution history logs is 90 days.
For more details refer: 
AWS Step Functions Limits Overview

## Product Versions
* Python 3 for AWS Lambda
* AWS Glue version 2

## Architecture

![image](https://user-images.githubusercontent.com/97115457/189493451-edd28319-2480-487e-b0e1-22742b9eccdb.png)
