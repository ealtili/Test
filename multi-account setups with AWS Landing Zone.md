
[Source](https://medium.com/@superluminar/tested-for-you-multi-account-setups-with-aws-landing-zone-b934154cfc78 "Permalink to Tested for you: multi-account setups with AWS Landing Zone")

# Multi-account setups with AWS Landing Zone

Sep 6

* * *

# Multi-account setups with AWS Landing Zone**

> _What are AWS Best Practices? How do I secure my workloads? How can I give teams maximum autonomy without compromising security or consistency? Which basic design should I use in AWS? How do I manage users and access rights? How does AWS Multi-Account work?_

These are some of the many questions we hear over and over again, particularly from customers who are new to AWS and struggling to negotiate their way through the vast array of concepts and services.  
Blog posts on best practices are to be found here and there — some of them posted by AWS itself — many of them current and many already outdated. But wouldn't it be great to have some kind of automated process that would provide a quick start, one that would allow you to get down to your actual work straight away?

We often see the AWS basic setup being 'clicked' for various reasons in the AWS web console — in other words, manually executed, making what ensues scarcely understandable or reproducible. Furthermore, good or best practices are often included in response to chance events; for example, an employee reads a blog post and links to the article in a dev chat as a 'we should do this' item. These items are often then left undone, as there is 'no time', it is 'not within scope', or the initial cost is excessive. Or they are done, but then 'clicked', whereupon the knowledge often remains in the clicker's head and in many cases left to gather dust.  
Previously there was no solution (that we were aware of) for the setup process, but this has changed with the AWS Landing Zone. Landing Zone is an 'AWS solution' that codifies current best practices and rolls them out in an automated manner.

* * *

### **What does Landing Zone promise?**

* Automated AWS multi-account setup
* Basic security guidelines
* Codified best practices (including updates directly from AWS); for example, automated CloudTrail setup and VPC/network design
* DevOps best practices: Infrastructure-as-Code with the use of codified templates and continuous delivery, whereby your own extensions can be rolled out globally.
* High adaptability owing to the use of templates
* Modularity
* Single Sign-On and central management of access rights (optional)

#### **First impressions**

#### **Installation**

Installation is performed using a CloudFormation template, also referred to as an 'initiation template', in which you select certain basic settings.

The initialisation process writes a config template to an S3 bucket. This serves as the source for a CodePipeline, which is also created using the initiation template.

This config can be adjusted later and is the 'single source of truth' for your Landing Zone. The CodePipeline runs with every change made to the config and applies the changes to the infrastructure. CloudFormation templates are a part of the configuration process in addition to the manifest, such as for virtual private clouds (VPCs) (and therefore also can be adjusted or extended).

For the first test, we recommend that you set the `BuildLandingZone` parameter to false to prevent the pipeline from immediately 'taking off'. This will give you a chance to inspect the generated config first and make any necessary adjustments to it before it begins to run. You may also want to set `LockStackSetsExecutionRole` to false first, as otherwise you may lose direct access to the sub-accounts. It is recommended, however, that you later set the parameter to true, as this restricts administrator role access in sub-accounts to the Landing Zone resources.

The basic setup creates the following accounts:

* Master: This is where you will find the AWS organisation, along with the CodePipeline, which rolls out the Landing Zone. Also found here are Single Sign-On (SSO) and a service catalogue for the 'Account Vending Machine' (AVM), which automates the process of creating new AWS accounts.
* Security: Here you can find roles that allow you to switch to other accounts or receive notifications of security incidents.
* Shared services: This is the place for Active Directory and other services that are used by all accounts.
* Logging: This is the central landing place for logs, such as CloudTrail audit logs.

![][1]

(source[: https://aws.amazon.com/answers/aws-landing-zone/][2])

**Account baseline**

A 'baseline' is provisioned in all accounts. In the default configuration, the baseline contains:

* CloudTrail setup (audit logs)
* AWS Config and a basic rule set ('Governance') used, for example, to send an alert if CloudTrail has been deactivated
* IAM password policy for IAM users
* Cross-account access from the Security account
* An optional VPC according to specifications
* Notifications and alarms, for example when root users log in

**Creating a new AWS account**

Once the AWS Landing Zone has been successfully set up, you can create a new AWS account through the AWS Service Catalogue, where an  
 `AWS-Landing-Zone-Account-Vending-Machine` product has been created. This now allows you to provision new accounts. The baseline is automatically created along with it.

![][3]

Selecting the product in the Service Catalogue

Once the product has been selected, the root user's email address can be selected for the new account, along with a name and the organisational unit. In addition, you can set whether and how a VPC is to be configured.

![][4]

Setting the parameters for the new account

* * *

### What we like

* Codification: The entire solution is documented in code, and there are no manual steps (with one exception: bending the CodePipeline to something other than S3 as a source takes just one click).
* Extensibility: Landing Zone provides a solid basis on which to roll out, for example, your own CloudFormation stacks across AWS accounts.
* Best practises directly from AWS: Here AWS has accumulated and codified their experience.
* Landing Zone is continually maintained and extended by AWS. No sooner than we run our test had a new version came out that fixed two bugs we had encountered. There are, as yet, no notifications or changelogs, however.
* Application to existing setups: It is fundamentally possible to migrate existing multi-account setups to AWS Landing Zone and thereby extend the benefits to existing setups as well.
* Idempotence: During the test we encountered several errors that brought everything to a standstill, for example in the pipeline. But after we fixed the code, we were able to re-initiate the pipeline, and things proceeded. We never ended up at an impasse. This is no doubt thanks to the consistent use of Lambda and Step Functions, which force a certain idempotence.

### What we don't like so much (yet)

We noticed the following, among other things, during the first tests:

* Complexity as a result of the large number of services used: This makes it difficult at times to understand what is happening. Here's an example: Service Catalog triggers CloudFormation, which has custom resources that trigger Lambda, which triggers a Step Functions state machine, which in turn calls inter alia CloudFormation StackSets. On the other hand, the consistent use of serverless components also guarantees that maintenance costs are lower.
* Not quite properly published yet: The solution is public but 'hidden'. It can be used all the same, and our first tests resulted in a fundamental 'production readiness'. Moreover, it is subject to the [Amazon Software License][5], which permits its use and alteration in the AWS context.
* The AWS SSO solution is limited to us-east-1, needs an Active Directory, and inherently cannot do multi-factor authentication.

### And the costs?

Landing Zone sets up a few resources by default that cost money. The most costly of them are:

* Active Directory Service
* AD Connector in the master account
* AWS Config Rules for each AWS account
* EC2 instance as Remote Desktop Gateway/JumpHost to connect to Active Directory

According to AWS, these add up to about $500 a month, a figure that can be substantially reduced by removing Active Directory.

### How do you test it on your own?

You can test AWS Landing Zone yourself. We recommend that you first run your tests in a newly created account with a new organisation so that you can gain some experience in a simulacrum environment. Landing Zone itself comes as a CloudFormation template and can therefore be installed with one click:

![][6]
[aws-landing-zone-initiation.template][10]
(in some cases, you may have to change the region in the URL if you do not wish to provision Landing Zone in `us-east-1`.)

Here are AWS' docs:

* [AWS Landing Zone Developer Guide][7]
* [AWS Landing Zone Implementation Guide][8]
* [AWS Landing Zone User Guide][9]
* [AWS Landing Zone Configuration file][14]

superluminar is an AWS Consulting Partner and will gladly help you automate or optimise your new or existing multi-account setup with AWS Landing Zone.

**Summary and outlook**

With Landing Zone, AWS provides us with a solid basis for secure and automated multi-account setups. In addition, the solution creates a positive impression on the strength of its extensibility, consistent use of Infrastructure-as-Code, continuous deployment, and updates from AWS.

We also discuss other aspects in the following articles:

* How do I extend Landing Zone, for example to roll out my own CloudFormation stacks globally across all accounts and keep them up to date?
* How do you get an existing AWS multi-account setup into Landing Zone? What do you have to watch out for?
* How do you adjust the baseline in order to, for example, start without Active Directory / SSO?
* How do you update Landing Zone with new releases? What happens during an update?
* How do I accomplish different account settings for different environments (e.g. dev/prod) or teams/organisational units?
* How does the built-in SSO solution work? What do you have to watch out for?


[Source](https://medium.com/@superluminar/extending-aws-landing-zone-a-real-world-example-63b8d46115dc "Permalink to Extending AWS Landing Zone: A real-world example – superluminar – Medium")

# Extending AWS Landing Zone: A real-world example 


Sep 14

* * *

# **Extending AWS Landing Zone: A real-world example**

## Now that we have taken [a basic look at AWS Landing Zone] and how it helps in the deployment of Enterprise AWS Setups, we would like to give you an example of how you can extend your Landing Zone.

In this tutorial we will be solving a problem from the real world: Some resources must be created once for each AWS account and/or region of an organization in order to enable additional functionalities.

Enabling Logging in the AWS API Gateway service is one such example: An IAM role must be created once for each AWS account and configured for each region with the API Gateway service so that the latter can then write logs and metrics to CloudWatch. This is very important especially for the purpose of debugging serverless applications, but it is not exactly intuitive ([set up API logging in API Gateway][11]).

Wouldn't it be nice if such basic settings were preconfigured directly for each of your organisation's AWS accounts, with no need for manual steps or 'how-to' wiki pages?

AWS Landing Zone offers basic functionality for precisely this purpose, allowing you to distribute your own AWS resources or settings across sub-accounts and regions. A crucial accumulation of knowledge can be codified and then distributed automatically. Development teams can begin their actual work more quickly, without having to go through the tedious and laborious process of configuring a basic setup.

To return to our example: For a basic setup of the API Gateway there are two things you need apart from an AWS Landing Zone setup:

* An IAM role, which is rolled out globally — and not by region — since IAM is a global service. For this we take the CloudFormation resource `AWS::IAM::Role`.
* The basic regional configuration of the API Gateway service so that the service can write logs and metrics to CloudWatch. For this you use the CloudFormation resource`AWS::ApiGateway::Account`.

Let us begin with the global IAM role, which must be present once per AWS account (but not for each region). For this you define your Landing Zone configuration in your manifest,`manifest.yaml`:
    
    
    - name: APIGatewayCloudWatchRole  
        baseline_products:  
          - AWS-Landing-Zone-Account-Vending-Machine  
        template_file: templates/superluminar/apigw-cloudwatch-role.template  
        deploy_method: stack_set

The CloudFormation template `templates/templates/superluminar/apigw-cloudwatch-role.template` is referenced in the manifest. This describes the global IAM role for the API Gateway service:
    
    
    AWSTemplateFormatVersion: 2010–09–09
    
    
    Resources:
    
    
    AWSTemplateFormatVersion: 2010-09-09  
      
    Resources:  
      ApiGwIamRole:  
        Type: AWS::IAM::Role  
        Properties:  
          RoleName: AmazonAPIGatewayPushToCloudWatch  
          ManagedPolicyArns:  
            - arn:aws:iam::aws:policy/service-role/AmazonAPIGatewayPushToCloudWatchLogs  
          AssumeRolePolicyDocument:  
            Version: 2012-10-17  
            Statement:  
              - Effect: Allow  
                Principal:  
                  Service: apigateway.amazonaws.com  
                Action: sts:AssumeRole

Once you have applied this change in our Landing Zone configuration, the CodePipeline starts and creates in each sub-account, via CloudFormation StackSets, a new CloudFormation stack from the template above:

![][12]

**CloudFormation StackSet with a stack instance for each sub-account**

Once the IAM role has been rolled out in each AWS account managed by Landing Zone, you still have to make it known for each such account to the API Gateway service so that the latter will use that role. You can do this using the following CloudFormation template:
    
    
    AWSTemplateFormatVersion: 2010-09-09  
      
    Resources:  
      ApiGwAccount:  
        Type: AWS::ApiGateway::Account  
        Properties:  
          CloudWatchRoleArn: !Sub "arn:aws:iam::${AWS::AccountId}:role/AmazonAPIGatewayPushToCloudWatch"

In this template you reference the IAM role that you created in the previous step.

You must also reference this template in the Landing Zone manifest and specify it as your own baseline:
    
    
    - name: APIGatewayAccountSetup  
        baseline_products:  
          - AWS-Landing-Zone-Account-Vending-Machine  
        depends_on:  
          - APIGatewayCloudWatchRole  
        template_file: templates/superluminar/apigw-account-setup.template  
        deploy_method: stack_set  
        regions:  
          - eu-central-1

In this case, a CloudFormation stack is rolled out in the region `eu-central-1` for each AWS account. From this point on, API gateways will be able to send logs to CloudWatch there.

* * *

### **How do updates work?**

From time to time, you may have to update a CloudFormation template. There is nothing special to watch out for here. Check in your changes, and the CodePipeline distributes the changes to each of the CloudFormation StackSets.

Even changes to the manifest are rolled out automatically. For example, you can add London to the regions in which your basic configuration from `APIGatewayAccountSetup` is rolled out by adding the region `eu-west-2`. To do so, you extend your list of regions:
    
    
    - name: APIGatewayAccountSetup  
        baseline_products:  
          - AWS-Landing-Zone-Account-Vending-Machine  
        depends_on:  
          - APIGatewayCloudWatchRole  
        template_file: templates/superluminar/apigw-account-setup.template  
        deploy_method: stack_set  
        regions:  
          - eu-central-1  
          - eu-west-2

The result is that the stack is now both rolled out in `eu-central-1`and newly rolled out in `eu-west-2`:

![][13]

**CloudFormation StackSet with a stack instance for each sub-account in two regions**

**And how about deleting?**

Deleting the code for a baseline resource in the manifest also deletes the corresponding CloudFormation stacks.

The result of a test in which we set out to delete individual regions from a list in a baseline was that those regions remained. We suspect that, with state convergence, region deletion has yet to be implemented in Landing Zone.

**Conclusion**

AWS Landing Zone provides a solid basis for centrally maintaining and rolling out 'baselines' — basic setups that are intended to be identical in each AWS sub-account — on an organisation-wide scale. This makes it easy to codify tried-and-tested good or best practices within a company and thereby avoid an uneven distribution of knowledge, or 'knowledge islands'.



[1]: https://cdn-images-1.medium.com/max/1600/1*gO4jrZrh0yQtghy_K4kr9Q.png
[2]: https://aws.amazon.com/answers/aws-landing-zone/
[3]: https://cdn-images-1.medium.com/max/1600/1*OPaSnO9PbJbHomcj_8OnGQ.png
[4]: https://cdn-images-1.medium.com/max/1600/1*DJ5X7HHJ5IDLXcN1Yl_UGQ.png
[5]: https://aws.amazon.com/asl/
[6]: https://cdn-images-1.medium.com/max/1600/1*xjXxq5Z1t_4SgoyPHexykg.png
[7]: https://s3.amazonaws.com/solutions-reference/aws-landing-zone/latest/aws-landing-zone-developer-guide.pdf
[8]: https://s3.amazonaws.com/solutions-reference/aws-landing-zone/latest/aws-landing-zone-implementation-guide.pdf
[9]: https://s3.amazonaws.com/solutions-reference/aws-landing-zone/latest/aws-landing-zone-user-guide.pdf
[10]: https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=aws-landing-zone-initiation&templateURL=https://s3.amazonaws.com/solutions-reference/aws-landing-zone/latest/aws-landing-zone-initiation.template
[11]: https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-logging.html
[12]: https://cdn-images-1.medium.com/max/1600/1*bVnX__GuIkAN6EIISwBWcg.png
[13]: https://cdn-images-1.medium.com/max/1600/1*57-oOb984nr9pKFLSnCmxw.png
[14]: https://s3.amazonaws.com/solutions-reference/aws-landing-zone/latest/aws-landing-zone-configuration.zip


