# S3 Lambda S3 using CDK
CDK program contains code to deploy two s3 buckets and one lambda function. Lambda function gets triggered from s3 bucket create object notifications, Lambda function code reads the object and resizes the image to thumbnail and stores the thumbnail in another s3 bucket

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

# Output
![alt text](https://github.com/mrparmsingh/aws/blob/main/servless-patterns/s3-lambda-s3-cdk/docs/output.png)

# Cleanup
Run the given command to delete the resources that were created. It might take some time for the CloudFormation stack to get deleted.

`cdk destroy`

