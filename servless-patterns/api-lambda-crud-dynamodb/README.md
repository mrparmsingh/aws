# API Gateway REST API to Lambda to DynamoDB using CDK
This pattern explains how to deploy a CDK application with Amazon API Gateway, AWS Lambda, and Amazon DynamoDB. When an HTTP POST request is made to the Amazon API Gateway endpoint, the AWS Lambda function is invoked and inserts an item into the Amazon DynamoDB table.

# Requirements
* Create an AWS account if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
* AWS CLI installed and configured
* Git Installed
* AWS Cloud Development Kit (AWS CDK) installed

# CDK Toolkit
The `cdk.json` file in the root of this repository includes instructions for the CDK toolkit on how to execute this program.

Specifically, it will tell the toolkit to use the mvn exec:java command as the entry point of your application. After changing your Java code, you will be able to run the CDK toolkit commands as usual (Maven will recompile as needed):

`$ cdk ls`

`<list all stacks in this program>`

`$ cdk synth`

`<cloudformation template>`

`$ cdk deploy`

`<deploy stack to your account>`

`$ cdk diff`

`<diff against deployed stack>`

# How it works
When an HTTP POST request is sent to the Amazon API Gateway endpoint, the AWS Lambda function is invoked and inserts an item into the Amazon DynamoDB table.

# Testing
Using the Amazon API Gateway console, select the API, Resources, and POST method execution. Click Test on the Client box and the Test button without a request body. The request should return a response message 'Successfully inserted data!'. If any errors occur, this view also shows logs from the invocation for troubleshooting. Navigate to the Amazon DynamoDB console to verify the item was added into the table.

# Cleanup
Run the given command to delete the resources that were created. It might take some time for the CloudFormation stack to get deleted.

`cdk destroy`
