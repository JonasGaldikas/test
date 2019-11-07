# test
new test project

## Serverless contact form with AWS Lambda and AWS SES

### Short TL;DR
* Configure AWS SES
* Build the API with the Serverless Framework
* Deploy the API to AWS Lambda


### Configure AWS SES
1. Verify an email address which will be used to send the emails.
2. Navigate to the AWS Console and search for Simple Email Service.
3. Once there press the Email Addresses link on the left side navigation. You'll see a big blue button called Verify a New Email Address. Press it and add your email address.
4. AWS will now send you a verification email to that address. Go ahead and verify it.

### Build the API with the Serverless Framework
1. Install the Serverless Framework
 `npm i -g serverless`
