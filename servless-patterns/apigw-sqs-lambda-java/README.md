# AWS API Gateway HTTP APIs to Amazon SQS for buffering
In this pattern, called "Queue based leveling", a serverless queue is introduced between your API Gateway and your workers, a Lambda function in this case.

The queue acts as a buffer to alleviate traffic spikes and ensure your workload can sustain the arriving load by buffering all the requests durably. It also helps downstream consumers to process the incoming requests at a consistent pace.

***Important***: this application uses various AWS services and there are costs associated with these services after the Free Tier usage - please see the AWS Pricing page for details. You are responsible for any AWS costs incurred. No warranty is implied in this example.

# Requirements
* Create an AWS account if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
* AWS CLI installed and configured
* Git Installed
* AWS Serverless Application Model (AWS SAM) installed

# Deployment Instructions
1. Create a new directory, navigate to that directory in a terminal and clone the GitHub repository:
  
    `https://github.com/mrparmsingh/aws`
    
3. Change directory to the pattern directory:

    `cd servless-patterns/apigw-sqs-lambda-java`
    
5. From the command line, use AWS SAM to deploy the AWS resources for the pattern as specified in the template.yml file:

    `sam deploy --guided`
    
5. During the prompts:
    * Enter a stack name
    * Enter the desired AWS Region
    * Allow SAM CLI to create IAM roles with the required permissions.
    
   Once you have run `sam deploy --guided` mode once and saved arguments to a configuration file (samconfig.toml), you can use `sam deploy` in future to use these defaults.
5. Note the outputs from the SAM deployment process. These contain the resource names and/or ARNs which are used for testing.
6. Build the maven project using: `mvn package`
7. Deploying jar to the lambda function using AWS CLI:
  
   `aws lambda update-function-code --function-name SQSLambdaFunction --zip-file fileb://SQSLambdaFunction-1.0-SNAPSHOT.jar`

# How it works
The API Gateway handles incoming requests, but instead of invoking the Lambda function directly, it stores them in an SQS queue which serves as a buffer. The Lambda functions (workers) can now process the requests in a batch manner (10 requests at a time).

# Testing
Run the following command to send an HTTP POST request to the HTTP APIs endpoint. Note, you must edit the {MyHttpAPI} placeholder with the URL of the deployed HTTP APIs endpoint. This is provided in the stack outputs.

`curl --location --request POST '{MyHttpAPI}/submit' --header 'Content-Type: application/json' --data-raw '{ "isMessageReceived": "Yes" }'`

Then open SQSLambdaFunction logs on Cloudwatch to notice a log that resembles to the message bellow, which means the message has gone through API Gateway, was pushed to SQS and then consumed by the Lambda function:

`2022-04-09T17:43:44.625+03:00	START RequestId: c5aa3ce2-125e-5ba4-a577-66298f8324db Version: $LATEST`

`2022-04-09T17:43:44.628+03:00	{ "isMessageReceived": "Yes" }`

`2022-04-09T17:43:44.630+03:00	END RequestId: c5aa3ce2-125e-5ba4-a577-66298f8324db`

# Cleanup
1. Delete the stack

    `aws cloudformation delete-stack --stack-name STACK_NAME`
    
3. Confirm the stack has been deleted

     `aws cloudformation list-stacks --query "StackSummaries[?contains(StackName,'STACK_NAME')].StackStatus"`
