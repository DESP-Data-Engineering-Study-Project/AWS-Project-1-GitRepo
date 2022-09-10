# Create Data Lake with Amazon S3, Lake Formation and Glue
In this workshop, you will keep two data sets sales and customers in Amazon S3. AWS Glue is used to catalog the data. You then use AWS Lake Formation to provide specific permission for the salesuser and customersuser users for the sales and customers tables. The users will use Amazon Athena to query the data as per the permission defined.

---
## Sections
- [Prerequisites](#Prerequisites)
- [Limitations](#Limitations)
- [Product Versions](#Product-Versions)
- [Architecture](#Architecture)
- [High level work flow](#High-level-work-flow)
- [Repository Structure](#Repository-Structure)
- [Deploy](#Deploy)
- [Sample Workflow Execution](#Sample-Workflow-Execution-and-Notification)

## Prerequisites 

* An active AWS account with admin and programmatic access.
* AWS CLI with AWS account configuration, so that you can create AWS resources by deploying CloudFormation stack.
* Amazon S3 bucket. 
* CSV dataset with correct schema ( attached is a sample csv file with correct schema and data type).
* Web browser.
* AWS Lake Formation console access
* AWS Glue console access.
* AWS Athena console access.
* AWS region Paris:- eu-west-3


## Limitations
AWS Lake Formation:

- Cloud Formation Template: Deploying stack not possible due to permission denied.

For more details refer:
- Issues: User is not authorized to perform: glue:CreateDatabase. 

AWS Glue Crawler:

- Cloud Formation Template: Deploying stack not possible due to permission denied.

For more details refer:
- Issues: User is not authorized to perform: glue:CreateCrawler. 


## Product Versions

* AWS Glue version 2
* AWS CLI version 1.25.59
* Python version 3.9.7
* Windows 10

## Architecture

![image](https://user-images.githubusercontent.com/97115457/189493451-edd28319-2480-487e-b0e1-22742b9eccdb.png)

## High level work flow

1. User uploads a csv file manually. The data sets are stored in Amazon S3.
2. Create two users. These two users will access data sets of the data lake with specific permissions configured.
3. Create IAM Role which is used by the AWS Glue crawler to catalog data for the data lake which will be stored in Amazon S3.
4. Create a database and configure it with Amazon S3 bucket location. A database is used to organize data catalog tables in the data lake.
5. Configure crawler to automatically create the data catalog tables in the data lake.
6. Crawler will use the IAM role which created earlier to create data catalog in the database.
7. Assign database permission for this role. After the permission configuration, create and run crawler to catalog the data.
8. The data lake is ready with the data catalog which gives access to the data stored in Amazon S3 bucket. It is time to configure the permissions as respective user.
9. Once logged in with respective user, make sure you choose the same region you used to setup the data lake. 
10. Then, go to Athena Service console and query data.


# Repository Structure
- template.yml - CloudFormation template file
 - lambda - This folder contains the following lambda function
    - start_crawler.py - Start the AWS Glue crawler
    - start_codebuild.py - Start AWS CodeBuild Project
    - s3object.py - Creates required directory structure inside S3 bucket


## Deploy
This pattern can be deployed through AWS CloudFormation template.

Follow the below step to deploy this pattern using CloudFormation template file [template.yml](template.yml) included in this repository.

1.	Clone the Repo
2.	Navigate to the Directory
3.	Update parameter.json file as follows - 
    - pS3BucketName - Unique bucket name. This bucket will be created to store all the dataset. As, S3 Bucket name is globally unique, provide a unique name.
    - pEmailforNotification - A valid email address to receive success/error notification.
    - pSourceFolder - Folder name (inside bucket created mentioned in 3.a) where source csv file will be uploaded inside 
    - pStageFolder - Folder name (inside bucket created mentioned in 3.a) used to staging area for AWS Glue Jobs 
    - pTransformFolder - Folder name (inside bucket created mentioned in 3.a) where transformed and portioned dataset will be stored 
    - pArchiveFolder - Folder name (inside bucket created mentioned in 3.a) where source csv file will be archived 
    - pErrorFolder - Folder name (inside bucket created mentioned in 3.a) where source csv file will be moved for any error 
    - pDatasetSchema - Valid schema of the source dataset. **Note** - For source dataset validation, cerberus python package has been used. For more information, refer [cerberus site](https://cerberus-sanhe.readthedocs.io/usage.html#type)


4. Execute the following AWS CLI command with pre-configured AWS CLI profile. 
    - Replace "Profile_Name" with a valid aws cli profile name
    - Replace "Stack_Name" with Proide a unique stack name
    - Replace "existing_bucket_name_in_the_same_region" with an existing S3 bcuket name in the same region where the stack will be deployed

    *aws cloudformation package --template-file template.yml --s3-bucket <**existing_bucket_name_in_the_same_region**> --output-template-file packaged.template --profile <**Profile_Name**>*

    *aws cloudformation deploy --stack-name <**Stack_Name**> --template-file packaged.template  --parameter-overrides file://parameter.json --capabilities CAPABILITY_IAM --profile <**Profile_Name**>*
5.	Check the progress of CloudFormation stack deployment in console and wait for it to finish

