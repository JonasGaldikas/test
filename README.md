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
1. Install the Serverless Framework `npm i -g serverless`
2. Once installed globally on your machine, the commands will be available to you from wherever in the terminal. But for it to communicate with your AWS account you need to `configure an IAM User`. Jump over [here for the explanation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html), then come back and run the command below, with the provided keys.
3. `serverless config credentials --provider aws --key xxxxxxxxxxxxxx --secret xxxxxxxxxxxxxx`
4. Now your Serverless installation knows what account to connect to when you run any terminal command.
