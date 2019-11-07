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
  * Once installed globally on your machine, the commands will be available to you from wherever in the terminal. But for it to communicate with your AWS account you need to `configure an IAM User`. Jump over [here for the explanation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html), then come back and run the command below, with the provided keys.
  * `serverless config credentials --provider aws --key xxxxxxxxxxxxxx --secret xxxxxxxxxxxxxx`
  * Now your Serverless installation knows what account to connect to when you run any terminal command.

2. Create a service
  * Create a new directory to house your Serverless application services. Fire up a terminal in there. Now you’re ready to create a new service.
  * `service`- It's where you define AWS Lambda functions, the events that trigger them and any AWS infrastructure resources they require, all in a file called **serverless.yml**.
  * Back in your terminal type: `serverless create --template aws-nodejs --path contact-form-api`
  * Create command will create a new **service**. We need to pick a runtime for the function. This is called the **template**. Passing in aws-nodejs will set the runtime to Node.js. The **path** will create a folder for the service.

3. Explore the service directory with a code editor
  * Open up the **contact-form-api** folder. There should be three files in there, but for now, we'll only focus on the **serverless.yml**.
  * Your **serverless.yml** will be full of boilerplate code and comments. Feel free to delete it all and paste this in.
  ```
  # serverless.yml

    service: contact-form-api

    custom:
    secrets: ${file(secrets.json)}

    provider:
    name: aws
    runtime: nodejs8.10
    deploymentBucket:
      name: enote-website-api
    stage: ${self:custom.secrets.NODE_ENV}
    region: eu-central-1
    environment:
      NODE_ENV: ${self:custom.secrets.NODE_ENV}
      EMAIL: ${self:custom.secrets.EMAIL}
      DOMAIN: ${self:custom.secrets.DOMAIN}
    iamRoleStatements:
      - Effect: "Allow"
        Action:
          - "ses:SendEmail"
        Resource: "*"

    functions:
    send:
      handler: handler.send
      events:
        - http:
            path: email/send
            method: post
            cors: true

  ```

  * The `functions` property lists all the functions in the service. We will only need one function though, to handle the sending of emails. The **handler** references which function it is.
  * `iamRoleStatements` , specify the Lambda has permission to trigger the Simple Email Service.
  * We also have a `custom` section at the top. This acts as a way to safely load environment variables into our service. They're later referenced by using `${self:custom.secrets.<environment_var>}` where the actual values are kept in a simple file called `secrets.json`.

4. Add the secrets file
  *  Add a secrets.json file and paste these values in.
  ```
  {
    "NODE_ENV":"dev",
    "EMAIL":"john.doe@mail.com",
    "DOMAIN":"*"
  }
  ```
  * While testing you can keep the domain as `'*'`, however, make sure to change this to your actual domain in production. The `EMAIL` field should contain the email you verified with AWS SES.

5. Write business logic
