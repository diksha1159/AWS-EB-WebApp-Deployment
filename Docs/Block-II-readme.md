
## Deploying a Node.js Web App on AWS Using Elastic Beanstalk and CDK
==================================================================

If you're looking to deploy a Node.js web app on AWS, Elastic Beanstalk is a fully managed service that makes it easy to deploy, run, and scale web applications. With Elastic Beanstalk, you don't have to worry about infrastructure setup, load balancing, or auto-scaling. Instead, you can focus on developing your application and let Elastic Beanstalk handle the rest.

In this project, I used the AWS Cloud Development Kit (CDK) to create an application that automates the infrastructure setup for deploying a Node.js web app using Elastic Beanstalk. CDK is an open-source software development framework that allows you to define cloud infrastructure in code and provision it through AWS CloudFormation. By defining infrastructure as code, it becomes easier to manage and maintain over time.

 CDK application had two main components: an Elastic Beanstalk application and an Elastic Beanstalk environment. An Elastic Beanstalk application is a logical container for our web application. It allows me to group my resources together and manage them as a single unit. My CDK application created a new Elastic Beanstalk application with a sample Node.js application that I can use to test my infrastructure.

The Elastic Beanstalk environment is the actual deployment of my web application. It consists of EC2 instances, an ELB, and other resources needed to run my application. My CDK application created a new Elastic Beanstalk environment and attached it to my application.

To summarize, my CDK application created an Elastic Beanstalk application and an Elastic Beanstalk environment. This infrastructure will allow me to easily deploy and manage my Node.js web app on AWS.

Conclusion
----------

Using Elastic Beanstalk and CDK can simplify the process of deploying a Node.js web app on AWS. Elastic Beanstalk allows me to focus on developing my application, while CDK makes it easy to define and provision infrastructure as code. If you're looking to deploy a Node.js web app on AWS, consider using Elastic Beanstalk and CDK to streamline the process.

---

>> now the actuall working, explaination & implementation of code

In this module, you will be developing a CDK (AWS Cloud Development Kit) application that will facilitate the creation of all the essential infrastructure required to deploy a Node.js web application on AWS Elastic Beanstalk.
Your goals in this module will be to create a straightforward CDK application, employ S3 Assets to upload a package to S3, and then utilize the CDK app to establish an app version, app environment, and Elastic Beanstalk CDK application.

---
Alright, let's dive into the implementation of our CDK app and make it both informative and fun!

First things first, we need to ensure that the specific version of CDK we install matches the dependencies we'll be using later on. So, let's create a new directory and initialize our CDK app there. We'll be using this as our base to create all the infrastructure we need.

Now, let's move on to creating the code for our resource stack! A resource stack, for those unfamiliar, is a collection of AWS resources that we'll be provisioning into a specific account. In our case, we've already configured the account in the prerequisite steps. Within this resource stack, we'll be creating some pretty neat resources:

S3 Assets: This little guy will help us zip up our application and upload it to S3. This gives our CDK app a way to locate the object and work its magic!
Elastic Beanstalk App: This is a logical grouping of Elastic Beanstalk components, including environments, versions, and configurations.
Elastic Beanstalk App Version: This is a specific, labeled iteration of deployable code for a web application. Each version points to an Amazon S3 object that contains the deployable code (our zip file!).
Instance profile and role: This is a container for an AWS Identity and Access Management (IAM) role that we'll use to pass role information to an Amazon EC2 instance when it starts up.
Elastic Beanstalk Environment: This is a collection of AWS resources running an application version. Each environment runs only one application version at a time.

With these resources, we'll be able to create a robust infrastructure that can handle our Node.js web application. So, let's get to it! Open up the /lib/cdk-eb-infra-stack.ts file and let's start coding!

----------------------

To deploy your web app, you need to pack it and upload it to Amazon S3 so that Elastic Beanstalk can deploy it. here we will see how to upload it automatically using a CDK constructor called S3 Assets.

First, you'll need to install the aws-s3-assets module by running "npm i @aws-cdk/aws-s3-assets". Then, in your lib/cdk-eb-infra-stack.ts file, add "import s3assets = require('@aws-cdk/aws-s3-assets');" to the top of the file.

Inside the stack, under "The code that defines your stack goes here" comment, add the following code:

```
const webAppZipArchive = new s3assets.Asset(this, 'WebAppZip', {
      path: `${__dirname}/../app.zip`,
});

```

This code uses the S3 Assets module to upload your web app's zip file located in the root of your CDK app to S3. Whenever you update the zip file and deploy this stack, the file in S3 will get updated too.

That's it! You can now easily upload your web app to S3 using CDK.


To create the Elastic Beanstalk application, application version, and environment, you need to install the Elastic Beanstalk module for CDK by running "npm i @aws-cdk/aws-elasticbeanstalk". Then, in your /lib/cdk-eb-infra-stack.ts file, add "import elasticbeanstalk = require('@aws-cdk/aws-elasticbeanstalk');" to the top of the file.

That's it! You're now ready to start deploying your web app using Elastic Beanstalk and CDK.


---

To create the Elastic Beanstalk app, you'll need to add the following code to your /lib/cdk-eb-infra-stack.ts file after the S3 Assets code. This code will create the application with the name MyWebApp in Elastic Beanstalk.

javascript

```javascript
// Create a ElasticBeanStalk app.
const appName = 'MyWebApp';
const app = new elasticbeanstalk.CfnApplication(this, 'Application', {
    applicationName: appName,
});
```

Next, you need to create an application version from the S3 asset you uploaded earlier. Add the following code to your file to create the app version using the S3 bucket name and S3 object key that S3 Assets and CDK will provide to this method.

php

```php
// Create an app version from the S3 asset defined earlier
const appVersionProps = new elasticbeanstalk.CfnApplicationVersion(this, 'AppVersion', {
    applicationName: appName,
    sourceBundle: {
        s3Bucket: webAppZipArchive.s3BucketName,
        s3Key: webAppZipArchive.s3ObjectKey,
    },
});
```

Before moving on, you'll want to make sure that the Elastic Beanstalk application exists before creating the app version. You can do this with CDK by adding the following code:

scss

```scss
// Make sure that Elastic Beanstalk app exists before creating an app version
appVersionProps.addDependsOn(app);
```

That's it! You've now created the Elastic Beanstalk application and app version using CDK.

---
---

Creating an Instance Profile for Elastic Beanstalk Environment
--------------------------------------------------------------

In order to create an Elastic Beanstalk environment, we need to set up an instance profile, which is essentially a container for an IAM role that passes role information to an EC2 instance upon startup. The IAM role will have a managed policy attached to it called AWSElasticBeanstalkWebTier, which allows permissions for uploading logs to S3 and debugging information to X-Ray.

To create the instance profile, we first need to install the IAM module in our CDK app using the command `npm i @aws-cdk/aws-iam`. Next, we import the dependency in our CDK stack using the line `import iam = require('@aws-cdk/aws-iam');`.

After the code that creates the application version, we add the following code to create the role and instance profile:

javascript

```javascript
const myRole = new iam.Role(this, `${appName}-aws-elasticbeanstalk-ec2-role`, {
    assumedBy: new iam.ServicePrincipal('ec2.amazonaws.com'),
});

const managedPolicy = iam.ManagedPolicy.fromAwsManagedPolicyName('AWSElasticBeanstalkWebTier')
myRole.addManagedPolicy(managedPolicy);

const myProfileName = `${appName}-InstanceProfile`

const instanceProfile = new iam.CfnInstanceProfile(this, myProfileName, {
    instanceProfileName: myProfileName,
    roles: [
        myRole.roleName
    ]
});
```

The first thing this code does is create a new IAM role (myRole) with Amazon EC2 as the trusted entity in the trust relationship policy, allowing EC2 instances to assume the role. We then add the managed policy AWSElasticBeanstalkWebTier to the role.

Next, we create an instance profile with the name `${appName}-InstanceProfile` and assign the previously created role to it. This completes the creation of the instance profile, which can now be used for the Elastic Beanstalk environment.

Overall, this code creates an instance profile that enables the EC2 instances to assume the necessary IAM role and utilize the managed policy AWSElasticBeanstalkWebTier for logging and debugging information in the Elastic Beanstalk environment.

---


---

To create the Elastic Beanstalk environment, you will need to provide some information about your infrastructure. The environment is a collection of AWS resources running an application version. You need to provide a name for the environment that will appear in the Elastic Beanstalk console. In this case, the environment is named MyWebAppEnvironment.

You will also need to give the application name that you got from the Elastic Beanstalk application definition earlier. The solution stack name is the name of the managed platform that Elastic Beanstalk provides for running web applications. You should choose the right software stack depending on the framework and platform you chose to develop your web app. In this case, we're using '64bit Amazon Linux 2 v5.6.1 running Node.js 14'.

The option settings attribute allows you to configure the Elastic Beanstalk environment to your needs, such as the instance profile, minimum and maximum sizes of the autoscaling group, and instance types. For this example, we're using configurations that are within the AWS Free Tier.

Finally, you need to provide the version label which is a reference to the application version that you just created.

To create the Elastic Beanstalk environment, use the following code and paste it in your stack definition file:

typescript

```typescript
const optionSettingProperties: elasticbeanstalk.CfnEnvironment.OptionSettingProperty[] = [
    {
        namespace: 'aws:autoscaling:launchconfiguration',
        optionName: 'IamInstanceProfile',
        value: myProfileName,
    },
    {
        namespace: 'aws:autoscaling:asg',
        optionName: 'MinSize',
        value: '1',
    },
    {
        namespace: 'aws:autoscaling:asg',
        optionName: 'MaxSize',
        value: '1',
    },
    {
        namespace: 'aws:ec2:instances',
        optionName: 'InstanceTypes',
        value: 't2.micro',
    },
];

const elbEnv = new elasticbeanstalk.CfnEnvironment(this, 'Environment', {
    environmentName: 'MyWebAppEnvironment',
    applicationName: app.applicationName || appName,
    solutionStackName: '64bit Amazon Linux 2 v5.6.1 running Node.js 14',
    optionSettings: optionSettingProperties,
    versionLabel: appVersionProps.ref,
});
```

This code will create an Elastic Beanstalk environment to run the application with the provided information.

---

Conclusion


In this module, you learned how to create the resources needed to deploy your application on Elastic Beanstalk automatically. In the next module, you will learn how to actually deploy your application to the cloud and how to make updates when necessary. This will enable you to easily manage your application and keep it up to date without needing to manually manage the underlying infrastructure.

----