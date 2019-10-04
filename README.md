# CI/CD with GitHub-Jenkins-AmazonS3-AmazonLambda-Amazon-DynamoDB

## Take a spreadsheet and build a pipeline to upload the data into AWS using tools like Lambda and Cloud Formation

|Stack|Name|
|:------:|:-------:|
|Provisioning|AWS CloudFormation|
|SCM|GitHub|
|ContinuousIntegration|Jenkins on AWS EC2|
|Continuous Deployment| Amazon S3|
|Post Deployment Processing| Amazon Lambda|
|Database| Amazon DynamoDB|
|Monitoring|Amazon Cloudwatch|
|Alerting|CloudWatch Alarms|

Here are the Major steps that are performed to complete the above task.

1. Created a "CSV" file and saved it in Github repository [https://github.com/navadeep0927/case-study/]

2. Launched a "Jenkins" by Bitnami AMI from Amazon Web Services Console

3. Installed necessary plugins[S3,Amazon EC2 plugin, Amazon S3 Bucket Credentials Plugin, AWS Lambda Plugin, Build Pipeline Plugin] to integrate Jenkins with AWS.

4. Configured AWS Access Key and Secret Access Key in Jenkins[Under Manage Jenkins]

5. Created a Webhook in Github and linked it with the "Jenkins pipeline job"  Under Build Triggers - Check the option "GitHub hook trigger for GITScm polling"

6. Scripted Jenkins pipeline job by making it "parameterised"

7. On AWS Console
   * Created a "Policy" using cloud formation template to provide full access to S3, cloudwatchlogs, dynamodb
   * Created a role : role_lambda_csv_reader and attach the policy in step (i) to it
   * Created a Lambda Function(lambda_csv_reader) and attach the role created in step (ii)
   * Create a trigger with S3, to pick the latest file uploaded to S3 after Jenkins build
   * Sripted a python program to read CSV file from S3 and upload the data in to DynamoDB[Need to create appropriate table in DynamoDB before executing this program]

8. A commit in the Github's master branch on the CSV file will trigger the Build pipeline, which in turn clones the Github, ListFiles, Lists buckets, copy to bucket, verify object copy and execute a python program to read the CSV file

9. Once the step-8 is done, the ".csv" file in S3 bucket gets triggered by the lambda function and stores the data in DynamoDB
