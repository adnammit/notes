# AWS LAMBDA

## Prerequisites
* latest SDK LTS installed
* AWS account with credentials set up
* AWS Toolkit extension


* `aws-lambda-tools-defaults.json` file
	* contains default settings for the Lambda function
	* `profile` must match the AWS profile you want to use
	* `function-handler` specifies the entry point as `{project}::{namespace}.{class}::{method}`

## Authenticating for Deployment
* you have profiles set up in `~/.aws/config` probably but the Lambda CLI doesn't support AWS SSO profiles -- you have to either export SSO credentials to the command line env or use static credentials ()
