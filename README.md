## Full-stack project Infrastructure (FE & BE)


### Frontend Infrastructure Overview

## Overview
This CloudFormation template deploys a complete infrastructure for hosting a React application with continuous integration and delivery (CI/CD) pipeline. The infrastructure includes:

- S3 bucket for hosting the React application
- CloudFront distribution for content delivery
- Lambda@Edge for request routing
- CodePipeline for automated deployments
- CodeBuild for building the React application
- IAM roles and policies for security

## Prerequisites
1. AWS Account
2. GitHub repository with React application
3. GitHub OAuth token
4. AWS CLI configured

## Parameters
- FrontendAppBucket: Name of the S3 bucket (must be globally unique)
- CloudFrontComment: Description for CloudFront distribution
- DefaultRootObject: Default document (typically index.html)
- CloudFrontEnabled: Enable/disable CloudFront distribution
- GitHubOwner: GitHub repository owner
- GitHubRepo: GitHub repository name
- GitHubBranch: Branch to deploy
- GitHubOAuthToken: GitHub OAuth token

## Deployment
1. Update parameter values in the template
2. Deploy using AWS CloudFormation:


aws cloudformation create-stack --stack-name frontend-stack --template-body file://frontend.yml --capabilities CAPABILITY_IAM


## Architecture
1. Source code changes pushed to GitHub
2. CodePipeline triggers on changes
3. CodeBuild builds React application
4. Built artifacts deployed to S3
5. CloudFront serves content with Lambda@Edge routing

## Security Features
- Private S3 bucket with CloudFront access only
- IAM roles with least privilege
- HTTPS enforcement
- No public S3 access

## Outputs
- CloudFrontURL: Application URL
- S3BucketName: Storage bucket name
- CodePipelineName: Pipeline name
- RoutingLambdaFunctionArn: Lambda function ARN

### Backend Infrastructure Overview

## AWS Resources
1. IAM Role (LambdaExecutionRole):
   - Allows Lambda to access DynamoDB and S3
   - Includes basic Lambda execution permissions
2. Lambda Function (BackendLambdaFunction):
   - Node.js 18.x runtime
   - 10-second timeout
   - Code loaded from S3 bucket
3. DynamoDB Table (UserDataTable):
   - Primary key: UserId (String)
   - Provisioned capacity: 5 read/write units
4. S3 Bucket (ImageBucket):
   - Private access
   - Blocked public access
5. API Gateway:
   - REST API with /backend endpoint
   - POST and GET methods
   - Lambda proxy integration
   - Production stage deployment

## Parameters
- DynamoDB table name
- S3 bucket name
- Lambda function name
- API Gateway name
- Lambda code location (S3 bucket and key)

## Outputs
- API Gateway URL
- S3 bucket name
- DynamoDB table name
- Lambda function name