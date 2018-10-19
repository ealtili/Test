
[Source](https://aws.amazon.com/answers/aws-landing-zone/ "Permalink to AWS Landing Zone – AWS Answers")

#  AWS Landing Zone

#  How can I save time setting up a multi-account AWS environment?

AWS Landing Zone is a solution that helps customers more quickly set up a secure, multi-account AWS environment based on AWS best practices. With the large number of design choices, setting up a multi-account environment can take a significant amount of time, involve the configuration of multiple accounts and services, and require a deep understanding of AWS services.

This solution can help save time by automating the set-up of an environment for running secure and scalable workloads while implementing an initial security baseline through the creation of core accounts and resources. It also provides a baseline environment to get started with a multi-account architecture, identity and access management, governance, data security, network design, and logging.

The AWS Landing Zone solution deploys an AWS Account Vending Machine (AVM) product for provisioning and automatically configuring new accounts. The AVM leverages [AWS Single Sign-On][46] (SSO) for managing user account access. This environment is customizable to allow customers to implement their own account baselines through a Landing Zone configuration and update pipeline.  



![webinar-icon1][47]

[Watch the AWS Landing Zone Webinar][48]


#### Request more information about AWS Landing Zone

[Contact AWS][49]

This solution is delivered by AWS Solutions Architects or Professional Services consultants to create a customized baseline of AWS accounts, networks, and security policies. You can request more information on the AWS Landing Zone solution by filling out the [signup form][49].  

* Multi-Account Structure
* Account Vending Machine
* User Access
* Security Baseline
* Notifications
* #### Multi-Account Structure __

 

The AWS Landing Zone solution includes four accounts:   

![landing-zone-implementation-architecture][50]

###  AWS Organizations Account

The AWS Landing Zone is deployed into an [AWS Organizations][51] account. This account is used to manage configuration and access to AWS Landing Zone managed accounts. The AWS Organizations account provides the ability to create and financially manage member accounts. It contains the AWS Landing Zone configuration Amazon Simple Storage Service (Amazon S3) bucket and pipeline, account configuration StackSets, [AWS Organizations Service Control Policies][52] (SCPS), and AWS Single Sign-On (SSO) configuration.

###  Shared Services Account

The Shared Services account is a reference for creating infrastructure shared services such as directory services. By default, this account hosts AWS Managed Active Directory for AWS SSO integration in a shared Amazon Virtual Private Cloud (Amazon VPC) that can be automatically peered with new AWS accounts created with the Account Vending Machine (AVM).

###  Logging Account

The Logging account contains a central Amazon S3 bucket for storing copies of all AWS CloudTrail and AWS Config log files in an audit log account.  

###  Security Account

The Security account creates auditor (read-only) and administrator (full-access) cross-account roles from a Security account to all AWS Landing Zone managed accounts. The intent of these roles is to be used by a company's security and compliance team to audit or perform emergency security operations in case of an incident.  

* #### Account Vending Machine __

 

The Account Vending Machine (AVM) is an AWS Landing Zone key component. The AVM is provided as an [AWS Service Catalog][53] product, which allows customers to create new AWS accounts in Organizational Units (OUs) preconfigured with an account security baseline, and a predefined network.  

![aws-landing-zone-account-vending-architecture][54]

    1. AWS Landing Zone leverages Service Catalog to grant administrators permissions to create and manage AWS Landing Zone products and end user's permissions to launch and manage AVM products.  

    2. The AVM uses launch constraints to allow end users to create new accounts without requiring account administrator permissions.  

    3. Optional products can be deployed using AVM, such as the Centralized Logging component.  

* #### User Access __

 

Providing least-privilege, individual user access to your AWS accounts is an essential, foundational component to AWS account management. The AWS Landing Zone solution lays the foundation for using AWS SSO for managing user access to your AWS accounts. This includes setting up [AWS Active Directory Connector][55] (AD Connector) in the AWS Organizations account connected to AWS Directory Service in a shared service account.  

![landing-zone-sso-diagram][56]

    1. AWS SSO creates a single-sign-on endpoint to federate user access to AWS accounts.  

    2. Active Directory (AD) Connector is a directory gateway used for redirecting directory requests running on-premises or in another AWS account to Microsoft Active Directory. 
    3. The solution also deploys AWS Microsoft AD to provide AWS SSO access to your user directory. Active Directory domain controllers are deployed into the Shared Services account to separate AD user management from AWS Landing Zone management functions. This makes it easier to leverage AD for applications or operating system management if desired.

* #### Security Baseline __

 

The AWS Landing Zone solution includes an initial security baseline that can be used as a starting point for establishing and implementing a customized account security baseline for your organization. By default, the initial security baseline includes the following settings:

###  [ AWS CloudTrail][57]

One CloudTrail trail is created in each account and configured to send logs to a centrally managed Amazon Simple Storage Service (Amazon S3) bucket in the logging account, and to AWS CloudWatch Logs in the local account for local operations (with a 14-day log group retention policy).  

###  [ AWS Config][58]

AWS Config is enabled and account configuration log files are stored in a centrally managed Amazon S3 bucket in the logging account.  

###  [ AWS Config Rules][59]

AWS Config rules are enabled for monitoring storage encryption (Amazon Elastic Block Store, Amazon S3, and Amazon Relational Database Service), AWS Identity and Access Management (IAM) password policy, root account multi-factor authentication (MFA), Amazon S3 public read and write, and insecure security group rules.

###  [ AWS Identity and Access Management][60]

AWS Identity and Access Management is used to configure an IAM password policy.

###  [ Cross-Account Access][61]

Cross-account access is used to configure audit and emergency security administrative access to AWS Landing Zone accounts from the security account.

###  [ Amazon Virtual Private Cloud (VPC)][62]

An Amazon VPC configures the initial network for an account. This includes deleting the default VPC in all regions, deploying the AVM requested network type, and network peering with the Shared Services VPC when applicable.

* #### Notifications __

 

The AWS Landing Zone solution configures [Amazon CloudWatch][63] alarms and events to send a notification on root account login, console sign-in failures, API authentication failures, and the following changes within an account: security groups, network ACLs, Amazon VPC gateways, peering connections, ClassicLink, Amazon Elastic Compute Cloud (Amazon EC2) instance state, large Amazon EC2 instance state, AWS CloudTrail, AWS Identity and Access Management (IAM) policies, and AWS Config rule compliance status.  

![landing-zone-notifications-diagram][64]

    1. The solution configures each account to send notifications to a local Amazon Simple Notification Service (Amazon SNS) topic.  

    2. An AWS Lambda function is automatically subscribed to this topic to forward all notifications to an aggregation Amazon SNS topic in the AWS Organizations account.  

    3. This architecture is designed to allow local administrators to subscribe to and receive specific account notifications.  


[47]: https://d1.awsstatic.com/events/aws-hosted-events/2018/APAC/sap-on-aws/webinar-icon1.3904006d61417be8a1902eacfbced0a33625b574.png "webinar-icon1"
[48]: https://pages.awscloud.com/Launch-AWS-Faster-using-Automated-Landing-Zones_0606-ENT_OD.html
[49]: https://pages.awscloud.com/AWS-Landing-Zone-Contact-Us.html
[50]: https://d1.awsstatic.com/aws-answers/answers-images/landing-zone-implementation-architecture.6bfa23d88aef1ce97035d0333f476898739697b9.png "landing-zone-implementation-architecture"
[51]: https://aws.amazon.com/organizations/
[52]: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_about-scps.html
[53]: https://aws.amazon.com/servicecatalog/
[54]: https://d1.awsstatic.com/aws-answers/answers-images/aws-landing-zone-account-vending-architecture.8058c098e8d7731c9e78bb32e22fa28e49b46b4a.png "aws-landing-zone-account-vending-architecture"
[55]: https://aws.amazon.com/directoryservice/
[56]: https://d1.awsstatic.com/aws-answers/answers-images/landing-zone-sso-diagram.4b3768156ae76ee555498522c186541da898be7b.png "landing-zone-sso-diagram"
[57]: https://aws.amazon.com/cloudtrail/
[58]: https://aws.amazon.com/config/
[59]: https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html
[60]: https://aws.amazon.com/iam/
[61]: https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html
[62]: https://aws.amazon.com/vpc/
[63]: https://aws.amazon.com/cloudwatch/
[64]: https://d1.awsstatic.com/aws-answers/answers-images/landing-zone-notifications-diagram.c27ae09e579bfd012f0bccb811bb270ce523d444.png "landing-zone-notifications-diagram"
