### Deploying our Web Application

Welcome to Module 3, where we'll be taking your web application to the cloud! You'll learn how to get your Node.js application ready for deployment and how to set up all the resources you need in AWS Elastic Beanstalk using AWS CDK. Plus, you'll learn how to update your application once it's up and running.

By the end of this module, you'll be able to package your application, build and deploy your CDK application, and keep your Node.js application up to date. Get ready to take your web app to the next level!

-------------

To deploy your Node.js application to AWS Elastic Beanstalk, you first need to package it into a zip file. In the previous module, you learned that we will be using the S3 Assets module in the AWS CDK to upload the zip file to S3.

To create the zip file, you need to run the following command in the root directory of your AWS CDK application under the directory "cdk-eb-infra":

```zip -r ../app.zip ./*```


This command will compress your Node.js application into a zip file and save it in the parent directory of your AWS CDK application. This zip file is what you will be uploading to S3 in the next steps.


---

Before you can use AWS CDK in your AWS account and region, you need to bootstrap it. This process involves provisioning initial resources, such as an Amazon S3 bucket for storing deployment files and IAM roles for permissions to perform deployments.

If this is your first time using AWS CDK in your account and region, you need to run the following command to bootstrap your environment:

javascript

```javascript
cdk bootstrap aws://ACCOUNT-NUMBER-1/REGION-1
```

For example, if your AWS account number is 123456789012 and your region is us-east-1, your command will look like this:

javascript

```javascript
cdk bootstrap aws://123456789012/us-east-1
```

Once you've run this command, you're ready to start deploying your AWS CDK applications!

---

Once you've packaged your Node.js application, placed it in the root of your CDK application directory, and bootstrapped your AWS account and region, you're ready to build and deploy your CDK application.

To build the CDK application, run the command "npm run build" in your terminal. This will create a compiled version of your application. If there are no errors, the build process will succeed.

Now it's time to deploy your CDK application to the cloud. Run the command "cdk deploy" in your terminal. This will create a CloudFormation stack that includes all the Elastic Beanstalk resources you created in the previous module, such as your Elastic Beanstalk application, application version, instance profile, and environment.

Because you created a new role, you will be asked to confirm changes in your account security level. Respond with "y" to proceed with the deployment. The deployment process takes a few minutes to complete.

When the deployment is finished, you will receive a message containing the ARN (Amazon Resource Name) of the CloudFormation stack that was created. If you check the CloudFormation Management console, you'll see two new stacks there.

The first stack is called "CdkEbInfraStack" and contains all the Elastic Beanstalk resources created in the previous module. The second stack, with a random string, was created by Elastic Beanstalk and contains all the resources the Elastic Beanstalk app needs to run, such as autoscaling groups, instances, Amazon CloudWatch alarms and metrics, load balancers, and security groups.


Viewing Your Web App in the Cloud
---------------------------------

To view your web app in the cloud, you need to find the URL of the Elastic Beanstalk environment called `MyWebAppEnvironment` in the AWS console. Once you find the URL, click on it to launch your web app.

Updating Your Web App Deployment
--------------------------------

If you want to make a change to your web app, and you want to redeploy it to the cloud, follow these steps:

1.  Make the change in the web app
2.  Package it in an `app.zip` file
3.  Place the `app.zip` file in the root directory of your CDK application
4.  Build the CDK project - `npm run build`
5.  Deploy the CDK project - `cdk deploy`

Now, you can verify that there is a new version of the Elastic Beanstalk app deployed. If you visit the web app URL, the new version will be deployed. This takes a bit of time; the console will indicate when the new deployment is complete.

---

Comclusions:

You're now able to package up your Node.js web app and deploy it with Elastic Beanstalk, while also deploying all the infrastructure as a CDK application. You're becoming quite the deployment wizard!
