
### Welcome to Storm Reply financial pipeline!

1. **Project's Title**\
 Storm Reply projects' financial tracking 

 2. **Project Description**\
 This application is used to enable the tracking of all projects of Storm Reply through Jira Board. It takes project tracker excel file and PDF invoices as an input and create/update seperate Jira ticket for each listed project to enable better visibility and tracking of all ongoing projects.<br />
 The application is developed in CDK with two stacks deployed. AWS services used in this project are AWS SES, labmda, S3, and SNS. All the services are deployed in eu-central-1 region with the exception of AWS SES which is deployed in us-west-2 region.

 3. **How to install and run the project**\
To deploy this project, follow the listed steps: <br />
  **a)** Verify domain in AWS SES following https://docs.aws.amazon.com/ses/latest/dg/receiving-email-verification.html<br />
  **b)**  Manually create a virtualenv on MacOS and Linux: $ python3 -m venv .venv<br />
  **c)**  Activate the virtualenv : $ source .venv/bin/activate or % .venv\Scripts\activate.bat incase of windows.<br />
  **d)** Go to cdk.json and configure recipient email address. The domain of this email address must be verified in AWS SES. We will send our excel sheet and pdf to this email so it is then received by AWS SES. Also configure an email address for SNS subscription in the same way. All updates will be sent to this email address for better tracking.<br />
  **e)** Deploy the CDK with following command: cdk deploy sandbox-storm-open-item-process-stack -c env=sandbox and cdk deploy sandbox-storm-open-item-ingestion-stack -c env=sandbox<br />
  Please note that AWS SES email receving feature is available in selected AWS regions, so check accordingly.
  **f)** You will receive SNS verification email to the subscribed account from AWS SNS. Verify it to enable receiving of AWS SNS alert at each invocation.<br />
  **g)** Once deployed, open Amazon SES from console and click on Email Receiving tab. Here click 'Set as active' to the SES rule.<br />
  **h)** Open AWS secret region and click stormOpenItem/sandbox/jiraApi. Here Add secret value of Jira_User and Jira_token.<br />
   If all steps are correctly performed, the project should be installed now.<br />
   Inorder to run the project, simply email the excel file of projects' tracker and pdf to finops@finance.poc.stormreply.cloud and monitor the Jira board for updated list of tickets.<br />
 You will also receive SNS alert for each invocation.

4.**Dataflow** <br />
**a)** User initiates the process by sending an email containing excel of the project's invoices to a verified entity.<br />
**b)** The email is received by AWS SES and it processes the raw email in MIME format by following actions in ruleset.<br /> 
**c)** SES delivers the raw email to an S3 bucket. This will invoke lambda function through event notification feature.<br /> 
**d)** The lambda function extracts email attachment and upload it to another s3 bucket.<br />
**e)** This s3 bucket is linked with two more lambdas: one lambda for pdf processing and another for xlsx processing. The lambdas will again be invoked by the event notification feature. The excel lambda will create seperate tickets for each invoicing details into the JIRA board, whereas the pdf lambda containing compilation of invoices will extract the relevant invoice details and attaches with the associated ticket.<br />
**f)** SNS alert will be sent to the subscribed email address.



5. **Current Configuration** <br />
This project contains two stacks: storm-open-item-process-stack and storm-open-item-ingestion-stack 

**Bucket-Names:**
sandbox-storm-open-item-bucket and sandbox-sesemails

**Region:**
eu-central-1 (with the exception of AWS SES which is in us-west-2)

**SNS-Topic:**
SANDBOXOpenItemTopic

**Lambdas:**
SANDBOXScanExcelLambda
SANDBOXScanPDFLambda
SANDBOXLambdaProcessemails

**AWS Secret:**
stormOpenItem/sandbox/jiraApi

**Active SES rule set:**
SANDBOXSESrule 

