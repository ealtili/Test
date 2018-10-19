# AWS Well-Architected Security Labs

## Introduction
This repository contains documentation and code in the format of hands-on labs to help you learn, measure, and build using architectural best practices. The labs are categorized into levels, where 100 is introductory, 200/300 is intermediate and 400 is advanced.

For more information about security on AWS visit [AWS Security](https://aws.amazon.com/security/) and read the [AWS Well-Architected Security whitepaper](https://d1.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf).

## Labs:
* [Level 100: AWS Account & Root User](100%20-%20AWS%20Account%20%26%20Root%20User)
* [Level 100: Basic Identity & Access Management User, Group, Role](100%20-%20Basic%20Identity%20%26%20Access%20Management%20User%2C%20Group%2C%20Role)
* [Level 200: Automated Deployment of Detective Controls](200%20-%20Automated%20Deployment%20of%20Detective%20Controls)
* [Level 200: Automated Deployment of IAM Groups and Roles](200%20-%20Automated%20Deployment%20of%20IAM%20Groups%20and%20Roles)
* [Level 200: Basic EC2 with WAF Protection](200%20-%20Basic%20EC2%20with%20WAF%20Protection)
* [Level 200: CloudFront with WAF Protection](200%20-%20CloudFront%20with%20WAF%20Protection)
* [Level 300: Arc325 Managing Multiple Accounts at Scale](arc325-multiple-accounts-workshop)
* [Level 400: AWS Landing Zone](AWS-Landing-Zone)


# Account Management

[Source](https://aws.amazon.com/answers/account-management/ "Permalink to Account Management – AWS Answers")

Lab [Level 300: Arc325 Managing Multiple Accounts at Scale](arc325-multiple-accounts-workshop)
###  Prescriptive guidance and best practices to help you set up and manage your AWS accounts

###  [ How should I manage multiple AWS accounts for billing purposes?][47]

Having multiple AWS accounts helps some customers more efficiently allocate costs and resources within their organizations. This brief provides account-level considerations, best practices, and high-level strategic guidance to help structure and manage multiple AWS accounts for billing purposes. [For more details »][47]

###  [ How should I manage multiple AWS accounts for security purposes?][48]

AWS customers often leverage multiple AWS accounts to help control access to their AWS resources. This brief provides account-level considerations, best practices, and high-level strategic guidance to help structure and manage multiple AWS accounts for security purposes. [For more details »][48]

###  [ How should I tag my AWS resources?][49]

This brief provides best practices for tagging AWS resources to make them easier to manage. It discusses common tag categories and strategies to help AWS customers implement a consistent and effective tagging approach for their AWS environment. [For more details »][49]

###  [ How do I receive notifications as I approach AWS service limits?][50]

AWS maintains service limits to help provide highly-available, reliable services to our customers. This answer provides best practices for monitoring service usage, and a solution that automatically sends an email notification when usage exceeds 80% of a limit. [For more details »][50]

###  [ Am I using the most cost-effective instances for my workloads?][51]

Amazon Elastic Compute Cloud (Amazon EC2) provides a wide selection of instance types and sizes, giving customers the flexibility to right size compute resources to meet their capacity needs at the lowest cost. This answer gives best practices for optimizing Amazon EC2 costs as well as an AWS solution that uses managed services to perform a right-sizing analysis of your Amazon EC2 instances. [For more details »][51]

###  [ How can I more easily monitor and analyze my estimated AWS costs?][52]

AWS billing reports provide information about resource usage and estimated costs to help customers monitor their usage and optimize costs. This answer provides AWS best practices for cost management and introduces a solution that enables you to visualize granular metrics from your detailed billing reports in a custom dashboard. [For more details »][52]

###  [ How can I manage my Amazon WorkSpaces billing models to optimize costs?][53]

AWS customers can benefit from monitoring their Amazon WorkSpaces usage to identify potential cost savings. This answer provides an AWS solution that automatically converts WorkSpaces to the most cost-effective billing model based on a user's individual usage. [For more details »][53]  

###  [ How do I monitor my AWS account activity in real-time?][54]

Monitoring AWS account activity can provide valuable insight into who is accessing your resources and how your resources are being used. This insight can help you make better-informed decisions that increase security and efficiency, facilitate compliance auditing, and optimize costs. This answer provides provides best practices for implementing an account-monitoring solution, as well as an overview of the Real-Time Insights on AWS Account Activity solution. [For more details »][54]

* * *

###  AWS Solution Briefs

[Multiple Account Billing Strategy][47]  
[Multiple Account Security Strategy][48]  
[Tagging Strategies][49]

###  AWS Solutions

[Amazon WorkSpaces Cost Optimizer][53]  
[ AWS Limit Monitor][50]  
[Cost Optimization Monitor][52]  
[Cost Optimization: EC2 Right Sizing][51]  
[Cross-Account Manager  
][48][Real-Time Insights on AWS Account Activity  
][54]  


[45]: https://aws.amazon.com/answers/
[46]: https://d1.awsstatic.com/Graphics/rule.8b3deaf243ba0c51cd290d635b55eb667aba05e2.png "Amazon Web Services"
[47]: https://aws.amazon.com/answers/account-management/aws-multi-account-billing-strategy/
[48]: https://aws.amazon.com/answers/account-management/aws-multi-account-security-strategy/
[49]: https://aws.amazon.com/answers/account-management/aws-tagging-strategies/
[50]: https://aws.amazon.com/answers/account-management/limit-monitor/
[51]: https://aws.amazon.com/answers/account-management/cost-optimization-ec2-right-sizing/
[52]: https://aws.amazon.com/answers/account-management/cost-optimization-monitor/
[53]: https://aws.amazon.com/answers/account-management/workspaces-cost-optimizer/
[54]: https://aws.amazon.com/answers/account-management/real-time-insights-account-activity/
[55]: https://docs.aws.amazon.com/forms/aws-doc-feedback?hidden_service_name=AWS%20Solutions&hidden_guide_name=Answers&hidden_file_name=account%20management%20landing%20page


***

#  AWS Landing Zone

[Level 400: AWS Landing Zone](AWS-Landing-Zone)

#  How can I save time setting up a multi-account AWS environment?

AWS Landing Zone is a solution that helps customers more quickly set up a secure, multi-account AWS environment based on AWS best practices. With the large number of design choices, setting up a multi-account environment can take a significant amount of time, involve the configuration of multiple accounts and services, and require a deep understanding of AWS services.

This solution can help save time by automating the set-up of an environment for running secure and scalable workloads while implementing an initial security baseline through the creation of core accounts and resources. It also provides a baseline environment to get started with a multi-account architecture, identity and access management, governance, data security, network design, and logging.

The AWS Landing Zone solution deploys an AWS Account Vending Machine (AVM) product for provisioning and automatically configuring new accounts. The AVM leverages AWS Single Sign-On (SSO) for managing user account access. This environment is customizable to allow customers to implement their own account baselines through a Landing Zone configuration and update pipeline.  

[Watch the AWS Landing Zone Webinar][56]

#### Request more information about AWS Landing Zone

[Contact AWS][57]

This solution is delivered by AWS Solutions Architects or Professional Services consultants to create a customized baseline of AWS accounts, networks, and security policies. You can request more information on the AWS Landing Zone solution by filling out the signup form.  

* Multi-Account Structure
* Account Vending Machine
* User Access
* Security Baseline
* Notifications
* #### Multi-Account Structure __

 

The AWS Landing Zone solution includes four accounts:   

![landing-zone-implementation-architecture][58]



[56]: https://pages.awscloud.com/Launch-AWS-Faster-using-Automated-Landing-Zones_0606-ENT_OD.html
[57]: https://pages.awscloud.com/AWS-Landing-Zone-Contact-Us.html
[58]: https://d1.awsstatic.com/aws-answers/answers-images/landing-zone-implementation-architecture.6bfa23d88aef1ce97035d0333f476898739697b9.png "landing-zone-implementation-architecture"


## License
Licensed under the Apache 2.0 and MITnoAttr License. 

Copyright 2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License"). You may not use this file except in compliance with the License. A copy of the License is located at

    http://aws.amazon.com/apache2.0/

or in the "license" file accompanying this file. This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
