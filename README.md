<!-- # Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify,
# merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE. -->

# Functionless URL Shortener
This app creates a url shortener without using any compute. All business logic is handled at the Amazon API Gateway level. The basic app will create an API gateway instance utilizing Cognito for authentication and authorization. It will also create a DyanmoDB table for data storage.

## The Backend

### Services Used
* [Amazon API Gatweay](https://aws.amazon.com/api-gateway/)
* [Amazon Cognito](https://aws.amazon.com/cognito/)
* [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)
* [Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/)*
* [Amazon S3](https://aws.amazon.com/s3/)*

*optional\**

### Requirements for deployment from CLI
* [AWS SAM CLI v0.37.0+](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)

### Deploying
Use the guided deployment the first time you deploy
```bash
sam deploy -g
```

#### CChoopse options
You can choose the default for all options. If you do not want caching or access logging, set them to `false`

```bash
## The name of the CloudFormation stack
Stack Name [URLShortener]:

## The region you want to deploy in
AWS Region [us-west-2]:

## The name of the application (lowercase no spaces)
Parameter AppName [shortener]: 

## The domain ame of your client
Parameter ClientDomain [http://localhost:8080]:

## Optional: enable a cache cluster with .5GB storage and a TTL of 300 seconds ( true | false )
Parameter AddCache [true]:

## Optional: enable access logging via Kinesis Firehose to an S3 bucket ( true | false )
Parameter AddAccessLogging [true]:

## Shows you resources changes to be deployed and require a 'Y' to initiate deploy
Confirm changes before deploy [y/N]: 

## SAM needs permission to be able to create roles to connect to the resources in your template
Allow SAM CLI IAM role creation [Y/n]:

## Save your choice for later deployments
Save arguments to samconfig.toml [Y/n]:
```

SAM will then deploy the cloudformation stack to your AWS account and provide required outputs for the included client.

After the first deploy you may redploy using `sam deploy` or redeploy with different options using `sam deploy -g`.

### Access Logging
I chose to use the firehose option to drop to S3 so you can add Kinesis data analytics if desired.

You can modify the log type and format in the API Gateway console under the proper stage. You can also modify the string in the `sam deploy -g` process. *Note: the sam config file might have issues storing some strng formats.*

*If you plan to use the client, take note of the output values at the end of the deploy*

## The Client
The client is a simple Vusejs application thatinterfaces with the backend and allows you to add and manage new URL for shortening. Instructions for running the client are found [here](./client/README.md).