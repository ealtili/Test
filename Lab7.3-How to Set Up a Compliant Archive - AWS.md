
[Source](https://aws.amazon.com/getting-started/projects/set-up-compliant-archive/?trk=gs_card "Permalink to How to Set Up a Compliant Archive - Amazon Web Services (AWS)")

# How to Set Up a Compliant Archive - Amazon Web Services (AWS)

####  [ Projects on AWS][45]

#  Set Up a Compliant Archive

###  Easily deploy and enforce compliance policies on archived data using Amazon Glacier

[ Get Started with the Project][46]

3 Steps  |  60 Minutes

####  [ Overview][47]

####  [ Services Used and Costs][48]

####  [ FAQs][49]

![aws-project_Build_Compliant_Archive][50]

[

![architectural-diagram][51]

][52]

![aws-project_compliant-archive_diagram][53]

Many enterprises must retain regulatory and compliance archives for extended durations, which traditionally require expensive, purpose-built hardware and can quickly drive up storage costs as the volume of data retention increases. Turning to cloud storage like [Amazon Glacier][54], for long term archiving allows companies to store data cost-effectively for years or even decades, offloading the administrative burdens of operating and scaling storage to Amazon Web Services. And with Amazon Glacier [Vault Lock][55], regulated industries such as financial services, legal firms, and healthcare organizations can feel secure about meeting compliance requirements, as Vault Lock allows them to set controls to meet compliance and regulatory objectives for electronic storage, such as SEC Rule 17a-4 (f).  

**What you'll accomplish:**

**Create a New Vault for Regulatory/Compliance Archives**

**Initiate Vault Lock**

**Test a Vault Lock Policy**

**Complete the Vault Lock Process**

**Set up Flexible Access Controls in the Vault Access Policy**

**What you'll need before starting:**  

**An AWS Account: **You will need an AWS account to provision resources for this project. [Sign up for AWS][56].

**IT Skill level:** Experience with storage and backup technologies, networking, virtualization, backup tools, and tape solutions is recommended, but not required.

**AWS Experience: **Basic knowledge of Amazon Glacier vaults and archives, and familiarity with AWS Identity and Access Management (IAM) are recommended to successfully complete this project.

**Archive upload tool:** The Glacier console does not support archive upload operations. You can use a number of 3rd party data management tools that are integrated with Amazon Glacier, such as Cloudberry or FastGlacier, to upload archives. Otherwise, you can use the AWS Command Line Interface (CLI) or write code to make upload requests using the AWS SDK. 

**Billing Estimates:**  

**Cost to complete project**: The estimated cost to complete this project is less than $1. This cost assumes that you follow the recommended configurations, and that you terminate all resources (i.e. delete Glacier test data) after a 30-day retention policy. Your use case may require different configurations that can impact your bill. Use the [Simple Monthly Calculator][57] to estimate costs tailored for your needs

**Monthly billing estimate**: The total cost of setting up and maintaining a compliant archive will vary depending on your archive size, location, number of requests made, and data transfer fees. Standard Glacier charges will apply, which start at $0.007/GB/month. To see a breakdown of the services used and their associated costs, see [Services Used and Costs][48].  

[ Get Started with the Project][46]

# Set Up a Compliant Archive

```
November 2016
```
## Contents

Introduction 1	

Step 1: Create an AWS Account 1	

Step 2: Create a New Vault 1	

Step 3: Specify a Compliance Policy for Your Vault 4	

```
Example 1: Deny Deletion Permissions for Archives Less Than 365 Days Old 5	
Example 2: Deny Deletion Permissions Based on a Tag 6	
```
Additional Resources 7	


## Introduction

This tutorial guides you through the following tasks.

- Create a New Vault for Regulatory/Compliance Archives
- Complete the Vault Lock Process
- Set up Flexible Access Controls in the Vault Access Policy

This tutorial is not meant for production environments and does not discuss
options in depth. After you complete the steps, you can find more in-depth
information in the Additional Resources section.

## Step 1: Create an AWS Account

If you already have an AWS account, you can skip this prerequisite and use your
existing account. To create an AWS account if you do not already have one:

1. Go to [http://aws.amazon.com/.](http://aws.amazon.com/.)
2. Choose **Create an AWS Account**.
3. Follow the online instructions.

Part of the sign-up procedure involves receiving a phone call and entering a PIN
using the phone keypad.

## Step 2: Create a New Vault

A vault is a container for storing archives. Your first step is to create a vault in
one of the supported AWS regions. In this getting started exercise, you create a
vault in the US West (Oregon) region. For a list of the AWS regions supported by
Amazon Glacier, go to Regions and Endpoints in the AWS General Reference.

You can create vaults programmatically or by using the Amazon Glacier console.
This section uses the console to create a vault.

1. Sign into the AWS Management Console and open the Amazon Glacier
    console at https://console.aws.amazon.com/glacier/.


2. Select a region from the region selector. In this getting started exercise, we
    use the US West (Oregon) region.
3. If you are using Amazon Glacier for the first time, click **Get started**.
    (Otherwise, click **Create Vault** .)
4. Enter **examplevault** as the vault name in the **Vault Name** field and
    then click **Next Step**. There are guidelines for naming a vault. For more
    information, see Creating a Vault in Amazon Glacier.
5. Select **Do not enable notifications**. For this getting started exercise,
    you will not configure notifications for the vault. If you wanted to have
    notifications sent to you or your application whenever certain Amazon
    Glacier jobs complete, you would select **Enable notifications and**
    **create a new SNS topic** or **Enable notifications and use an**


```
existing SNS topic to set up Amazon Simple Notification Service
(Amazon SNS) notifications. In subsequent steps, you upload an archive
and then download it using the high-level API of the AWS SDK. Using the
high-level API does not require that you configure vault notification to
retrieve your data.
```
6. If the region and vault name are correct, then click **Submit**.
7. Your new vault is listed on the **Amazon Glacier Vaults** page.


## Step 3: Specify a Compliance Policy for

## Your Vault

Amazon Glacier Vault Lock allows you to easily deploy and enforce compliance
controls for individual Amazon Glacier vaults with a vault lock policy. You can
specify controls such as “write once read many” (WORM) in a vault lock policy
and lock the policy from future edits. Once locked, the policy can no longer be
changed.

Amazon Glacier enforces the controls set in the vault lock policy to help achieve
your compliance objectives, for example, for data retention. You can deploy a
variety of compliance controls in a vault lock policy using the AWS Identity and
Access Management (IAM) policy language. For more information about vault
lock policies, see Amazon Glacier Access Control with Vault Lock Policies.

A vault lock policy is different than a vault access policy. Both policies govern
access controls to your vault. However, a vault lock policy can be locked to
prevent future changes, providing strong enforcement for your compliance
controls. You can use the vault lock policy to deploy regulatory and compliance
controls, which typically require tight controls on data access. In contrast, you
use a vault access policy to implement access controls that are not compliance
related, temporary, and subject to frequent modification. Vault lock and vault
access policies can be used together. For example, you can implement time-based
data retention rules in the vault lock policy (deny deletes), and grant read access
to designated third parties or your business partners (allow reads).

Locking a vault takes two steps:

- Initiate the lock by attaching a vault lock policy to your vault, which sets
    the lock to an in-progress state and returns a lock ID. While in the in-
    progress state, you have 24 hours to validate your vault lock policy before
    the lock ID expires.


- Use the lock ID to complete the lock process. If the vault lock policy
    doesn't work as expected, you can abort the lock and restart from the
    beginning. For information on how to use the Amazon Glacier API to lock
    a vault, see Locking a Vault by Using the Amazon Glacier API.

Here are two examples of vault lock policies you might want to use with your
vault.

**Note:** For the purpose of this exercise we recommend you use "7 days" versus
"365 days" so that you can easily check the policy and delete the archive, without
incurring charges after 7 days.

### Example 1: Deny Deletion Permissions for Archives

### Less Than 365 Days Old

Suppose that you have a regulatory requirement to retain archives for up to one
year before you can delete them. You can enforce that requirement by
implementing the following Vault Lock policy. The policy denies
the glacier:DeleteArchive action on the examplevault vault if the
archive being deleted is less than one year old. The policy uses the Amazon
Glacier-specific condition key ArchiveAgeInDays to enforce the one-year
retention requirement.

#### {

"Version":"2012- 10 - 17",
"Statement":[
{
"Sid": "deny-based-on-archive-age",
"Principal": "*",
"Effect": "Deny",
"Action": "glacier:DeleteArchive",
"Resource": [
"arn:aws:glacier:us-west-
2:123456789012:vaults/examplevault"
],
"Condition": {
"NumericLessThan" : {
"glacier:ArchiveAgeInDays" : "365"
}
}
}
]


#### }

### Example 2: Deny Deletion Permissions Based on a

### Tag

Suppose that you have a time-based retention rule that an archive can be deleted
if it is less than a year old. At the same time, suppose that you need to place a
legal hold on your archives to prevent deletion or modification for an indefinite
duration during a legal investigation. In this case, the legal hold takes precedence
over the time-based retention rule specified in the Vault Lock policy.

To put these two rules in place, the following example policy has two statements:

1. The first statement denies deletion permissions to everyone, locking the
    vault. This lock is performed by using the LegalHold tag.
2. The second statement grants deletion permissions when the archive is less
    than 365 days old. But even when archives are less than 365 days old, no
    one can delete them because the vault has been locked by the first
    statement.

{
"Version":"2012- 10 - 17",
"Statement":[
{
"Sid": "no-one-can-delete-any-archive-from-vault",
"Principal": "*",
"Effect": "Deny",
"Action": [
"glacier:DeleteArchive"
],
"Resource": [
"arn:aws:glacier:us-west-
2:123456789012:vaults/examplevault"
],
"Condition": {
"StringLike": {
"glacier:ResourceTag/LegalHold": [
"true",
""
]
}
}


#### },

#### {

"Sid": "you-can-delete-archive-less-than- 1 - year-
old",
"Principal": "*",
"Effect": "Allow",
"Action": [
"glacier:DeleteArchive"
],
"Resource": [
"arn:aws:glacier:us-west-
2:123456789012:vaults/examplevault"
],
"Condition": {
"NumericLessThan": {
"glacier:ArchiveAgeInDays": "365"
}
}
}
]
}

## Additional Resources

We recommend that you continue to learn more about the concepts introduced in
this guide with the following resources:

- Amazon Glacier Vault Lock
- Abort Vault Lock (DELETE lock-policy)
- Complete Vault Lock (POST lockId)
- Get Vault Lock (GET lock-policy)
- Initiate Vault Lock (POST lock-policy)

* * *

##  Additional Resources

[Compliance assessment on Amazon Glacier Vault Lock performed by Cohasset Associates, Inc][58]  

Compliance assessment on Amazon Glacier Vault Lock performed by Cohasset Associates, Inc to use with your regulator or Designated Examining Authority (DEA) in support of the SEC 17a-4(f)(2)(i) and CFTC 1.31(c) requirement  

[Whitepaper: AWS Risk and Compliance][59]  

This document includes a basic approach to evaluating AWS controls and provides information to assist customers with integrating control environments.  


[46]: https://d1.awsstatic.com/Projects/P4113791/aws-project_set-up-compliant-archive.pdf
[47]: https://aws.amazon.com/getting-started/projects/set-up-compliant-archive/
[48]: https://aws.amazon.com/getting-started/projects/set-up-compliant-archive/services-costs/
[49]: https://aws.amazon.com/getting-started/projects/set-up-compliant-archive/faq/
[50]: https://d1.awsstatic.com/Projects/P4113791/aws-project_Build_Compliant_Archive.13f39587aa5f14891f4da8f4eb4a4521731fe4d6.png "aws-project_Build_Compliant_Archive"
[51]: https://d1.awsstatic.com/Projects/build_sharepoint_site/architectural-diagram.0dea57198909e89748c8e1f7b05bd26d6b9d2808.png "architectural-diagram"
[52]: https://aws.amazon.com#aws-element-modal-82ea1b2c-d5ef-4a7c-8266-0c03b6520f60
[53]: https://d1.awsstatic.com/Projects/P4113791/aws-project_compliant-archive_diagram.ed5176c02a02532b3fc32beb0d07aec875edfbe8.png "aws-project_compliant-archive_diagram"
[54]: https://aws.amazon.com/glacier/
[55]: https://aws.amazon.com/glacier/details/#Vault_Lock
[56]: https://portal.aws.amazon.com/gp/aws/developer/registration/index.html
[57]: https://calculator.s3.amazonaws.com/index.html
[58]: https://d0.awsstatic.com/whitepapers/Amazon-GlacierVaultLock_CohassetAssessmentReport.pdf
[59]: https://d0.awsstatic.com/whitepapers/compliance/AWS_Risk_and_Compliance_Whitepaper.pdf
[60]: https://aws.amazon.com/getting-started/
[61]: https://portal.aws.amazon.com/gp/aws/developer/registration/index.html?nc2=h_ct
[62]: https://twitter.com/awscloud?nc1=f_so_tw
[63]: https://www.facebook.com/amazonwebservices?nc1=f_so_fb
[64]: https://aws.amazon.com/podcasts/aws-podcast/
[65]: https://www.twitch.tv/aws
[66]: https://aws.amazon.com/blogs/
[67]: https://aws.amazon.com/new/feed/
[68]: https://pages.awscloud.com/communication-preferences?trk=homepage
[69]: https://aws.amazon.com/what-is-cloud-computing/?nc1=f_cc
[70]: https://aws.amazon.com/caching/?nc1=f_cc
[71]: https://aws.amazon.com/nosql/?nc1=f_cc
[72]: https://aws.amazon.com/devops/what-is-devops/?nc1=f_cc
[73]: https://aws.amazon.com/docker/?nc1=f_cc
[74]: https://aws.amazon.com/products/?nc1=f_cc
[75]: https://aws.amazon.com/solutions/case-studies/?nc1=f_cc
[76]: https://aws.amazon.com/economics/?nc1=f_cc
[77]: https://aws.amazon.com/architecture/?nc1=f_cc
[78]: https://aws.amazon.com/security/?nc1=f_cc
[79]: https://aws.amazon.com/new/?nc1=f_cc
[80]: https://aws.amazon.com/whitepapers/?nc1=f_cc
[81]: https://aws.amazon.com/about-aws/events/?nc1=f_cc
[82]: https://aws.amazon.com/about-aws/sustainability/?nc1=f_cc
[83]: https://press.aboutamazon.com/press-releases/aws?nc1=f_cc
[84]: https://aws.amazon.com/about-aws/in-the-news/
[85]: https://aws.amazon.com/resources/analyst-reports/?nc1=f_cc
[86]: https://aws.amazon.com/legal/?nc1=f_cc
[87]: https://aws.amazon.com/websites/?nc1=f_dr
[88]: https://aws.amazon.com/business-applications/?nc1=f_dr
[89]: https://aws.amazon.com/backup-restore/?nc1=f_dr
[90]: https://aws.amazon.com/disaster-recovery/?nc1=f_dr
[91]: https://aws.amazon.com/archive/?nc1=f_dr
[92]: https://aws.amazon.com/devops/?nc1=f_dr
[93]: https://aws.amazon.com/serverless/?nc1=f_dr
[94]: https://aws.amazon.com/big-data/?nc1=f_dr
[95]: https://aws.amazon.com/hpc/?nc1=f_dr
[96]: https://aws.amazon.com/mobile/?nc1=f_dr
[97]: https://aws.amazon.com/digital-marketing/?nc1=f_dr
[98]: https://aws.amazon.com/gaming/?nc1=f_dr
[99]: https://aws.amazon.com/digital-media/?nc1=f_dr
[100]: https://aws.amazon.com/government-education/?nc1=f_dr
[101]: https://aws.amazon.com/health/?nc1=f_dr
[102]: https://aws.amazon.com/financial-services/?nc1=f_dr
[103]: https://aws.amazon.com/windows/?nc1=f_dr
[104]: https://aws.amazon.com/retail/?nc1=f_dr
[105]: https://aws.amazon.com/power-and-utilities/?nc1=f_dr
[106]: https://aws.amazon.com/oil-and-gas/?nc1=f_dr
[107]: https://aws.amazon.com/automotive/?nc1=f_dr
[108]: https://aws.amazon.com/blockchain/?nc1=f_dr
[109]: https://aws.amazon.com/manufacturing/?nc1=f_dr
[110]: https://aws.amazon.com/tools/?nc1=f_dr
[111]: https://aws.amazon.com/java/?nc1=f_dr
[112]: https://aws.amazon.com/javascript/?nc1=f_dr
[113]: https://aws.amazon.com/php/?nc1=f_dr
[114]: https://aws.amazon.com/python/?nc1=f_dr
[115]: https://aws.amazon.com/ruby/?nc1=f_dr
[116]: https://aws.amazon.com/net/
[117]: https://aws.amazon.com/marketplace/ref=mkt_ste_dev_resources_ftr?nc1=f_dr
[118]: https://aws.amazon.com/usergroups/?nc1=f_dr
[119]: https://aws.amazon.com/premiumsupport/?nc1=f_dr
[120]: http://status.aws.amazon.com/?nc1=f_dr
[121]: https://forums.aws.amazon.com/index.jspa?nc1=f_dr
[122]: https://aws.amazon.com/faqs/?nc1=f_dr
[123]: https://aws.amazon.com/documentation/?nc1=f_dr
[124]: https://aws.amazon.com/articles/?nc1=f_dr
[125]: https://aws.amazon.com/quickstart/?nc1=f_dr
[126]: https://aws.amazon.com/console/?nc1=f_m
[127]: https://console.aws.amazon.com/billing/home?nc1=f_m
[128]: https://pages.awscloud.com/communication-preferences.html
[129]: https://portal.aws.amazon.com/gp/aws/developer/account/index.html?action=edit-aws-profile&nc1=f_m
[130]: https://portal.aws.amazon.com/gp/aws/developer/account/index.html?action=edit-payment-method&nc1=f_m
[131]: https://aws.amazon.com/iam/?nc1=f_m
[132]: https://console.aws.amazon.com/iam/home?action=access-key&nc1=f_m
[133]: https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase
[134]: https://aws.amazon.com/contact-us/?nc1=f_m
[135]: https://aws.amazon.com/careers/?nc1=f_hi
[136]: http://www.amazon.com/
[137]: https://aws.amazon.com/de/?nc1=f_ls
[138]: https://aws.amazon.com/?nc1=f_ls
[139]: https://aws.amazon.com/es/?nc1=f_ls
[140]: https://aws.amazon.com/fr/?nc1=f_ls
[141]: https://aws.amazon.com/it/?nc1=f_ls
[142]: https://aws.amazon.com/pt/?nc1=f_ls
[143]: https://aws.amazon.com/ru/?nc1=f_ls
[144]: https://aws.amazon.com/th/?nc1=h_ls
[145]: https://aws.amazon.com/jp/?nc1=f_ls
[146]: https://aws.amazon.com/ko/?nc1=f_ls
[147]: https://aws.amazon.com/cn/?nc1=f_ls
[148]: https://aws.amazon.com/tw/?nc1=f_ls
[149]: https://aws.amazon.com/terms/?nc1=f_pr
[150]: https://aws.amazon.com/privacy/?nc1=f_pr
[151]: https://amazonwebservices.d2.sc.omtrdc.net/b/ss/awsamazonalldev2/1/H.25.1--NS/0

  