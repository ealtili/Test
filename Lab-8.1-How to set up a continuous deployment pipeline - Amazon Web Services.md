
[Source](https://aws.amazon.com/getting-started/tutorials/continuous-deployment-pipeline/ "Permalink to How to set up a continuous deployment pipeline - Amazon Web Services")

# How to set up a continuous deployment pipeline - Amazon Web Services

In this project, you will learn how to set up a continuous integration and continuous delivery (CI/CD) pipeline on AWS. A pipeline helps you automate steps in your software delivery process, such as initiating automatic builds and then deploying to Amazon EC2 instances. You will use AWS CodePipeline, a service that builds, tests, and deploys your code every time there is a code change, based on the release process models you define. Use CodePipeline to orchestrate each step in your release process. As part of your setup, you will plug other AWS services into CodePipeline to complete your software delivery pipeline. This guide will show you how to create a very simple pipeline that pulls code from a source repository and automatically deploys it to an Amazon EC2 instance.  


![setup-cicd-pipeline2][44]


#  Set up a Continuous Deployment Pipeline using AWS CodePipeline

In this tutorial, you will learn how to create an automated software release pipeline that deploys a live sample app. You will create the pipeline using [AWS CodePipeline][45], a service that builds, tests, and deploys your code every time there is a code change. You will use your GitHub account, an [Amazon Simple Storage Service (S3)][46] bucket, or an [AWS CodeCommit][47] repository as the source location for the sample app's code. You will also use [AWS Elastic Beanstalk][48] as the deployment target for the sample app. Your completed pipeline will be able to detect changes made to the source repository containing the sample app and then automatically update your live sample app.

Continuous deployment allows you to deploy revisions to a production environment automatically without explicit approval from a developer, making the entire software release process automated.

Everything done in this tutorial is [free tier][49] eligible.

#### Manage Your AWS Resources

[Sign in to the Console][43]

##  Step 1: Create a deployment environment

Your continuous deployment pipeline will need a target environment containing virtual servers, or Amazon EC2 instances, where it will deploy sample code. You will prepare this environment before creating the pipeline.

* * *

a. To simplify the process of setting up and configuring EC2 instances for this tutorial, you will spin up a sample environment using AWS Elastic Beanstalk. Elastic Beanstalk lets you easily host web applications without needing to launch, configure, or operate virtual servers on your own. It automatically provisions and operates the infrastructure (e.g. virtual servers, load balancers, etc.) and provides the application stack (e.g. OS, language and framework, web and application server, etc.) for you.

* [Click here to open the AWS Elastic Beanstalk console.][50]
* * *

b. Choose _PHP _from the drop-down menu and then click **Launch Now**.

_Note:_ If you have created an Elastic Beanstalk application before, click: **Create New Application** on the upper-right corner. Name your application and create a new _web server environment_. Select _PHP_ as your platform and _Single Instance_ as your environment type. If you are planning to remote login to your instances, select a key pair. Otherwise, leave default values for the remaining options and create the environment for your continuous deployment pipeline.  

[

![continuous-delivery-pipeline-1][51]

][52]

(click to zoom)  

![continuous-delivery-pipeline-1][51]

* * *

c. Elastic Beanstalk will begin creating a sample environment for you to deploy your application to. It will create an Amazon EC2 instance, a security group, an Auto Scaling group, an Amazon S3 bucket, Amazon CloudWatch alarms, and a domain name for your application.

_Note_: This will take several minutes to complete.

[

![continuous-delivery-pipeline-1c][53]

][54]

(click to zoom)  

![continuous-delivery-pipeline-1c][53]

##  Step 2: Get a copy of the sample code

In this step, you will retrieve a copy of the sample app's code and choose a source to host the code. The pipeline takes code from the source and then performs actions on it.

You can use one of three options as your source: a GitHub repository, an Amazon S3 bucket, or an AWS CodeCommit repository. Select your preference and follow the steps below:  

 

* GitHub
* Amazon S3
* AWS CodeCommit
* #### GitHub __

a. If you would like to use your GitHub account:

    * Visit our GitHub repository containing the sample code at . 
    * Fork a copy of the repository to your own GitHub account by clicking the **Fork **button in the upper-right corner.  

[

![continuous-delivery-pipeline-2a][55]

][56]

(click to zoom)  

![continuous-delivery-pipeline-2a][55]

* #### Amazon S3 __

a. If you plan to use Amazon S3 as your source, you will retrieve the sample code from the AWS GitHub repository, save it to your computer, and upload it to an Amazon S3 bucket.

    * Visit our GitHub repository containing the sample code at 
    * Click the _dist_ folder.

[

![continuous-delivery-pipeline-2a1][57]

][58]

(click to zoom)  

![continuous-delivery-pipeline-2a1][57]

* * *

b. Save the source files to your computer:

    * Click the file named _aws-codepipeline-s3-aws-codedeploy_linux.zip _
    * Click** View Raw**.
    * Save the sample file to your local computer.

[

![continuous-delivery-pipeline-2b2][59]

][60]

(click to zoom)  

![continuous-delivery-pipeline-2b2][59]

* * *

c. [Click here to open the Amazon S3 console][61] and create your Amazon S3 bucket:

    * Click **Create Bucket**
    * **Bucket Name:** type a unique name for your bucket, such as _awscodepipeline-demobucket-variables_.  All bucket names in Amazon S3 must be unique, so use one of your own, not one with the name shown in the example.
    * **Region: **In the drop-down, select the region where you will create your pipeline, such as _US Standard_. 
    * Click **Create**.

[

![continuous-delivery-pipeline-2d][62]

][63]

(click to zoom)  

![continuous-delivery-pipeline-2d][62]

* * *

d. The console displays the newly created bucket, which is empty.

    * Click **Properties.**
    * Expand _Versioning _and select **Enable Versioning. **When versioning is enabled, Amazon S3 saves every version of every object in the bucket.

[

![continuous-delivery-pipeline-2g][64]

][65]

(click to zoom)  

![continuous-delivery-pipeline-2g][64]

* * *

e. You will now upload the sample code to the Amazon S3 bucket: 

    * Click **Upload.**
    * Follow the on-screen directions to upload the .zip file containing the sample code you downloaded from GitHub.

[

![continuous-delivery-pipeline-2f][66]

][67]

(click to zoom)  

![continuous-delivery-pipeline-2f][66]

* #### AWS CodeCommit __

a. If you plan to use AWS CodeCommit as your source, you will retrieve the sample code from the AWS GitHub repository, save it to your computer, and upload it to an AWS CodeCommit repository.

    * Visit our GitHub repository containing the sample code at 
    * Select the _dist_ folder.

[

![continuous-delivery-pipeline-2a1][57]

][68]

(click to zoom)  

![continuous-delivery-pipeline-2a1][57]

* * *

b. Save the source files to your computer:

    * Select the file named _aws-codepipeline-s3-aws-codedeploy_linux.zip. _
    * Select** View Raw**.
    * Save the sample file to your local computer.

[

![continuous-delivery-pipeline-2b2][59]

][69]

(click to zoom)  

![continuous-delivery-pipeline-2b2][59]

* * *

d. [Click here to open the AWS CodeCommit console][70] and select **Get Started.**

[

![continuous-delivery-pipeline-2b3][71]

][72]

(click to zoom)  

![continuous-delivery-pipeline-2b3][71]

* * *

e. On the **Create new repository** page: 

    * **Repository name: **enter _PipelineRepo_.  

    * Select **Create repository**.

[

![continuous-delivery-pipeline-2c3][73]

][74]

(click to zoom)  

![continuous-delivery-pipeline-2c3][73]

* * *

f. Connect to your repository and then push a copy of the sample file to it.  For instructions, see [Connect to an AWS CodeCommit Repository][75].

##  Step 3: Create your pipeline

In this step, you will create and configure a simple pipeline with two actions: source and deploy. You will provide CodePipeline with the locations of your source repository and deployment environment.

* * *

a. [Click here to open the AWS CodePipeline console.][76]

* On the Welcome page, click **Create pipeline**. 
* If this is your first time using AWS CodePipeline, an introductory page appears instead of Welcome. Click **Get Started**.

[

![continuous-delivery-pipeline-3][77]

][78]

(click to zoom)

![continuous-delivery-pipeline-3][77]

* * *

b. On the _Step 1: Name_ page:

* **Pipeline name: **enter the name for your pipeline, _DemoPipeline. _
* Click **Next step**.

_Note:_ After you create a pipeline, you cannot change its name.

[

![continuous-delivery-pipeline-3b][79]

][80]

(click to zoom)

![continuous-delivery-pipeline-3b][79]

* * *

c. On the _Step 2: Source_ page, select the location of the source you selected and follow the steps below:

* GitHub
* Amazon S3
* AWS CodeCommit
* #### GitHub __

**Source Provider: **GitHub

    * In the** Connect to GitHub **section, click **Connect to GitHub**.
    * A new browser window will open to connect you to GitHub. If prompted to sign in, provide your GitHub credentials. 
    * You will be asked to authorize application access to your account. Choose **Authorize application**.

[

![continuous-delivery-pipeline-3c][81]

][82]

(click to zoom)  

![continuous-delivery-pipeline-3c][81]

* * *

Specify the repository and branch:

    * **Repository:** In the drop-down list, choose the GitHub repository you want to use as the source location for your pipeline. Click the forked repository in your GitHub account containing the sample code called _aws-codepipeline-s3-aws-codedeploy_linux_. 
    * **Branch:** In the drop-down list, choose the branch you want to use, _master._
    * Click **Next step**.

[

![continuous-delivery-pipeline-3e][83]

][84]

(click to zoom)  

![continuous-delivery-pipeline-3e][83]

* #### Amazon S3 __

**Source provider**: Amazon S3.

    * **Amazon S3 location:** Type the name of the Amazon S3 bucket you created followed by the sample file you copied to that bucket (_aws-codepipeline-s3-aws-codedeploy_linux.zip_). For example, if you named your bucket _awscodepipeline-demobucket-variable_, then you would type: _s3://awscodepipeline-demobucket-variable/aws-codepipeline-s3-aws-codedeploy_linux.zip._
    * Click **Next step.**

[

![continuous-delivery-pipeline-3c2][85]

][86]

(click to zoom)  

![continuous-delivery-pipeline-3c2][85]

* #### AWS CodeCommit __

**Source provider: **_AWS CodeCommit._

    * **Repository name: **Choose the name of your AWS CodeCommit repository. 
    * **Branch name:** Choose the name of the branch that contains the sample file.  

    * Click **Next step.**

[

![continuous-delivery-pipeline-3c3][87]

][88]

(click to zoom)  

![continuous-delivery-pipeline-3c3][87]

* * *

d. A true continuous deployment pipeline requires a build stage, where code is compiled and unit tested. CodePipeline lets you plug your preferred build provider into your pipeline. However, in this tutorial you will skip the build stage.

* In _Step 3: Build _page, choose **No Build.**
* Click **Next step.**

 

[

![continuous-delivery-pipeline-3f][89]

][90]

(click to zoom)

![continuous-delivery-pipeline-3f][89]

* * *

e. In the _Step 4: Beta _page:

* Deployment provider: Click **AWS Elastic Beanstalk. **
* Application name: Click **My First Elastic Beanstalk Application. **
* Environment name: Click **Default-Environment.**
* Click **Next step**.

_Note:_ The name "Beta" is simply the name given by default to this stage of the pipeline, just as "Source" was the name given to the first stage of the pipeline.

[

![continuous-delivery-pipeline-3g][91]

][92]

(click to zoom)

![continuous-delivery-pipeline-3g][91]

* * *

f. In the _Step 5: Service Role_ page:

* Service Role: Click **Create role.**
* You will be redirected to an IAM console page that describes the AWS-CodePipeline-Service role that will be created for you. Click **Allow**
* After you create the role, you are returned to the _Step 5: Service Role_ page where _AWS-CodePipeline-Service_ appears in Role name. Click **Next step**.

_Note:_ Service role creation is only required the first time you create a pipeline in AWS CodePipeline. If a service role has already been created, you will be able to choose it from the drop-down list of roles. Because the drop-down list will display all IAM service roles associated with your account, if you choose a name different from the default, be sure that the name is recognizable as the service role for AWS CodePipeline.

[

![continuous-delivery-pipeline-3fa][93]

][94]

(click to zoom)

![continuous-delivery-pipeline-3fa][93]

##  Step 4: Activate your pipeline to deploy your code

In this step, you will launch your pipeline. Once your pipeline has been created, it will start to run automatically. First, it detects the sample app code in your source location, bundles up the files, and then move them to the second stage that you defined. During this stage, it passes the code to Elastic Beanstalk, which contains the EC2 instance that will host your code. Elastic Beanstalk handles deploying the code to the EC2 instance.  

* * *

a. In the _Step 6: Review_ page, review the information and click **Create pipeline**.

[

![continuous-delivery-pipeline-4][95]

][96]

(click to zoom)

![continuous-delivery-pipeline-4][95]

* * *

b. After your pipeline is created, the pipeline status page appears and the pipeline automatically starts to run. You can view progress as well as success and failure messages as the pipeline performs each action.

To verify your pipeline ran successfully, monitor the progress of the pipeline as it moves through each stage. The status of each stage will change from _No executions yet _to _In Progress_, and then to either _Succeeded _or _Failed_. The pipeline should complete the first run within a few minutes.

[

![continuous-delivery-pipeline-4a][97]

][98]

(click to zoom)

![continuous-delivery-pipeline-4a][97]

* * *

c. In the status area for the Beta stage, click **AWS Elastic Beanstalk**. 

[

![continuous-delivery-pipeline-4a2][99]

][100]

(click to zoom)

![continuous-delivery-pipeline-4a2][99]

* * *

d. The AWS Elastic Beanstalk console opens with the details of the deployment.

* Click the environment you created earlier, called **Default-Environment**. 

[

![continuous-delivery-pipeline-4b][101]

][102]

(click to zoom)

![continuous-delivery-pipeline-4b][101]

* * *

e. Click the URL that appears in the upper-right part of the page to view the sample website you deployed.

[

![continuous-delivery-pipeline-6e][103]

][104]

(click to zoom)

![continuous-delivery-pipeline-6e][103]

##  Step 5: Commit a change and then update your app

In this step, you will revise the sample code and commit the change to your repository. CodePipeline will detect your updated sample code and then automatically initiate deploying it to your EC2 instance via Elastic Beanstalk. 

Note that the sample web page you deployed refers to AWS CodeDeploy, a service that automates code deployments. In CodePipeline, CodeDeploy is an alternative to using Elastic Beanstalk for deployment actions. Let's update the sample code so that it correctly states that you deployed the sample using Elastic Beanstalk.

* GitHub
* Amazon S3
* AWS CodeCommit
* #### GitHub __

a. Visit your own copy of the repository that you forked in GitHub.

    * Open _index.html_
    * Select the **Edit **icon.

[

![continuous-delivery-pipeline-5b1][105]

][106]

(click to zoom)  

![continuous-delivery-pipeline-5b1][105]

* * *

b. Update the webpage by copying and pasting the following text on line 30: 
    
        You have successfully created a pipeline that retrieved this source application from GitHub and deployed it to one Amazon EC2 instance using AWS Elastic Beanstalk. You're one step closer to practicing continuous deployment!
    
    
    

[

![continuous-delivery-pipeline-5c][107]

][108]

(click to zoom)  

![continuous-delivery-pipeline-5c][107]

* * *

c. Commit the change to your repository.

[

![continuous-delivery-pipeline-5c1][109]

][110]

(click to zoom)  

![continuous-delivery-pipeline-5c1][109]

* #### Amazon S3 __

a. On your desktop, visit the zip file you downloaded called _aws-codepipeline-s3-aws-codedeploy_linux.zip_.

* * *

b. Edit the sample web app code:

    * Extract _index.html_ from the zip file and open it using your preferred text editor. 
    * Update the header text that comes after "Congratulations!" so that it reads:
    
        "You have successfully created a pipeline that retrieved this source application from Amazon S3 and deployed it to one Amazon EC2 instance using AWS Elastic Beanstalk. You're one step closer to practicing continuous deployment!"
    
    
    

    * Copy the updated index.html file back into _aws-codepipeline-s3-aws-codedeploy_linux.zip_ and replace the older version of index.html.

[

![continuous-delivery-pipeline-6b2][111]

][112]

(click to zoom)  

![continuous-delivery-pipeline-6b2][111]

* * *

c. Reupload the edited file to your Amazon S3 bucket:

    * Return to the S3 bucket that you created earlier. 
    * Upload the updated _aws-codepipeline-s3-aws-codedeploy_linux.zip_ file into the bucket. 

_Note_: Because you enabled versioning when you first created the S3 bucket, S3 will save a copy of every version of your files.

[

![continuous-delivery-pipeline-6c2][113]

][114]

(click to zoom)  

![continuous-delivery-pipeline-6c2][113]

* #### AWS CodeCommit __

a. Visit the zip file containing the sample code you downloaded called_ aws-codepipeline-s3-aws-codedeploy_linux.zip._  

* * *

b. Edit the sample web app code: 

 

    * Extract _index.html_ from the zip file and open it using your preferred text editor. 
    * Then update the header text that comes after "Congratulations!" so that it reads:  

    
        You have successfully created a pipeline that retrieved this source application from AWS CodeCommit and deployed it to one Amazon EC2 instance using AWS Elastic Beanstalk. You're one step closer to practicing continuous deployment!
    
    
    

[

![continuous-delivery-pipeline-6b2][111]

][115]

(click to zoom)  

![continuous-delivery-pipeline-6b2][111]

* * *

c. Commit and push the updated zip file to your CodeCommit repository.  

* * *

d. Return to your pipeline in the CodePipeline console. In a few minutes, you should see the Source change to blue, indicating that the pipeline has detected the changes you made to your source repository. Once this occurs, it will automatically move the updated code to Elastic Beanstalk.

* After the pipeline status displays _Succeeded_, in the status area for the Beta stage, click **AWS Elastic Beanstalk**.

[

![continuous-delivery-pipeline-4a2][99]

][116]

(click to zoom)

![continuous-delivery-pipeline-4a2][99]

* * *

e. The AWS Elastic Beanstalk console opens with the details of the deployment. Select the environment you created earlier, called **Default-Environment**.

[

![continuous-delivery-pipeline-4b][101]

][117]

(click to zoom)

![continuous-delivery-pipeline-4b][101]

* * *

f. Click the URL that appears in the upper-right part of the page to view the sample website again.  Your text has been updated automatically through the continuous deployment pipeline!

 

[

![continuous-delivery-pipeline-6e][103]

][118]

(click to zoom)

![continuous-delivery-pipeline-6e][103]

##  Step 6: Clean up your resources

To avoid future charges, you will delete all the resources you launched throughout this tutorial, which includes the pipeline, the Elastic Beanstalk application, and the source you set up to host the code.    

* * *

a. First, you will delete your pipeline:

* In the pipeline view, click **Edit**. 
* Click **Delete**.
* Type in the name of your pipeline and click **Delete**.

[

![continuous-delivery-pipeline-5d-6][119]

][120]

(click to zoom)

![continuous-delivery-pipeline-5d-6][119]

* * *

b. Second, delete your Elastic Beanstalk application: 

* Visit the Elastic Beanstalk console. 
* Click **Actions.**
* Then click** Terminate Environment**.

[

![continuous-delivery-pipeline-6][121]

][122]

(click to zoom)

![continuous-delivery-pipeline-6][121]

* Amazon S3
* AWS CodeCommit
* #### Amazon S3 __

c. If you created an S3 bucket for this tutorial, delete the bucket you created:

    * Visit the S3 console. 
    * Right-click the bucket name and select **Delete Bucket**. 
    * When a confirmation message appears, enter the bucket name and then click **Delete**.

[

![continuous-delivery-pipeline-6d][123]

][124]

(click to zoom)

![continuous-delivery-pipeline-6d][123]

* #### AWS CodeCommit __

c. If you created an AWS CodeCommit repository for this tutorial, visit the CodeCommit console and delete the repository that you created:

    * [Click here to open the AWS CodeCommit repository][70].
    * Click the repository you created.

[

![continuous-delivery-pipeline-7a][125]

][126]

(click to zoom)

![continuous-delivery-pipeline-7a][125]

* * *

c. In the navigation pane, select **Settings.**

    * Click **Delete Repository**.
    * A confirmation window will pop up. Type the name of your repository and click **Delete**.

[

![continuous-delivery-pipeline-7b][127]

][128]

(click to zoom)

![continuous-delivery-pipeline-7b][127]

#  Congratulations!

You have successfully created an automated software release pipeline using AWS CodePipeline! Using CodePipeline, you created a pipeline that uses GitHub, Amazon S3, or AWS CodeCommit as the source location for application code and then deploys the code to an Amazon EC2 instance managed by AWS Elastic Beanstalk. Your pipeline will automatically deploy your code every time there is a code change. You are one step closer to practicing continuous deployment!  

##  Next Steps

Now that you have learned to create a simple pipeline using AWS CodePipeline, you can learn more by visiting the following resources.

* Create a more advanced, four-stage pipeline by [following this guide][129]. This pipeline uses a GitHub repository for your source, a Jenkins build server to build and test the project, and an AWS CodeDeploy application to deploy the built code to a staging server.  
* Quickly spin up a four-stage pipeline with a Jenkins build server by [using our Pipeline Starter Kit][130].
* Learn more about [continuous delivery][131].

[44]: https://d1.awsstatic.com/Projects/CICD%20Pipeline/setup-cicd-pipeline2.5cefde1406fa6787d9d3c38ae6ba3a53e8df3be8.png "setup-cicd-pipeline2"
[45]: https://aws.amazon.com/codepipeline/
[46]: https://aws.amazon.com/s3/
[47]: https://aws.amazon.com/codecommit/
[48]: https://aws.amazon.com/elasticbeanstalk/
[49]: https://aws.amazon.com/free/
[50]: https://us-east-1.console.aws.amazon.com/elasticbeanstalk
[51]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-1.286706cf35c41e3a5e08fb133d91747ddd16a3f8.png "continuous-delivery-pipeline-1"
[52]: https://aws.amazon.com#aws-element-modal-a5d5d44a-3995-4b35-bc4c-b930df387a6c
[53]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-1c.fb9b902019a21a382caa0afd67cd2d3d32787f6d.png "continuous-delivery-pipeline-1c"
[54]: https://aws.amazon.com#aws-element-modal-18c3be9c-b0b8-4b88-8413-dc75c4e871ea
[55]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2a.f094c15e8cb4cdbe57ab759afdc6f8bb9323c11d.png "continuous-delivery-pipeline-2a"
[56]: https://aws.amazon.com#aws-element-modal-a498a307-daa6-4c4e-8cb2-9566a6b14361
[57]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2a1.0fa47e564b3de9532ebb2ddf1b835de7fd300f77.png "continuous-delivery-pipeline-2a1"
[58]: https://aws.amazon.com#aws-element-modal-3b4b7656-ec1d-4e05-8e00-5bbf5a86c193
[59]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2b2.c6dd21fba4736376de049003349007b528efec17.png "continuous-delivery-pipeline-2b2"
[60]: https://aws.amazon.com#aws-element-modal-d283ccd4-8782-4426-9bc8-2c9037f6757b
[61]: https://console.aws.amazon.com/s3/
[62]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2d.38e09e6dbd11fed0a3cddf772358f9f3668a4ab2.png "continuous-delivery-pipeline-2d"
[63]: https://aws.amazon.com#aws-element-modal-4758d4f4-5788-4635-9f4b-1d30db57b0ec
[64]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2g.5ce68c572d0934bff48bbb80cf8e0249dca4be88.png "continuous-delivery-pipeline-2g"
[65]: https://aws.amazon.com#aws-element-modal-29701a7c-8a41-4161-b42e-27f3d152363a
[66]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2f.a7f5de538c24705396a64cd4cb706bbaebe50897.png "continuous-delivery-pipeline-2f"
[67]: https://aws.amazon.com#aws-element-modal-ee4a49ad-df16-4b88-8919-d38f7060d3c7
[68]: https://aws.amazon.com#aws-element-modal-f9169a51-a885-4ca6-9f90-a61e0b70f3bd
[69]: https://aws.amazon.com#aws-element-modal-cbb16d9d-86e6-46c3-be72-4f228ced7cbe
[70]: https://console.aws.amazon.com/codecommit
[71]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2b3.98ab5b0b9c0db7526c114f54d9da6c5f2bcee79f.png "continuous-delivery-pipeline-2b3"
[72]: https://aws.amazon.com#aws-element-modal-5f6c31d4-c222-459d-92ba-11ce8b4725fd
[73]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-2c3.f503f85e032354354b8c08d5a2795b60c3eee170.png "continuous-delivery-pipeline-2c3"
[74]: https://aws.amazon.com#aws-element-modal-d22eea9b-1472-4050-8f08-3cf01630a4d5
[75]: http://docs.aws.amazon.com/codecommit/latest/userguide/how-to-connect.html
[76]: http://console.aws.amazon.com/codepipeline
[77]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3.a6828f6deb20180f3661afdfeabd5a88b3bbc1dd.png "continuous-delivery-pipeline-3"
[78]: https://aws.amazon.com#aws-element-modal-ce6c4964-5e0a-41b6-af12-b43cbe851ace
[79]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3b.f436e164c1d8748498b421d51bedf8d52352e6a9.png "continuous-delivery-pipeline-3b"
[80]: https://aws.amazon.com#aws-element-modal-6234f28d-4ff9-4b2f-8d30-ce02a2c71298
[81]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3c.dca154abcdf69643f48201f4e773711281e912b0.png "continuous-delivery-pipeline-3c"
[82]: https://aws.amazon.com#aws-element-modal-de6bd2de-8ae6-4e72-87cd-66803f9b2567
[83]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3e.3974816427d470de71f60aa9754c382f0e5e5985.png "continuous-delivery-pipeline-3e"
[84]: https://aws.amazon.com#aws-element-modal-5ee52742-1109-4852-8c79-5b4849dd4bea
[85]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3c2.491252bf27525e128adde4c3458732157f415204.png "continuous-delivery-pipeline-3c2"
[86]: https://aws.amazon.com#aws-element-modal-9d884318-c2dc-4de1-9407-025ddea57160
[87]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3c3.045e849f9de57e843ab753d1f15d53bcd2d064be.png "continuous-delivery-pipeline-3c3"
[88]: https://aws.amazon.com#aws-element-modal-3d531b79-9c8b-4139-8385-6f8ac0e11fe3
[89]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3f.86f260b16d6cf34ab4d1b62e02966fa48569cf71.png "continuous-delivery-pipeline-3f"
[90]: https://aws.amazon.com#aws-element-modal-94fe0479-89b9-49e2-8aa3-d28a5eb1f8fe
[91]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3g.c9a7391ef64d98d9991dd4c8182b937823a875e6.png "continuous-delivery-pipeline-3g"
[92]: https://aws.amazon.com#aws-element-modal-e97b9cf7-fb83-4f55-881a-d4a56e1c9e25
[93]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-3fa.a495181bd6bfaa94a88c0b48905eab33a0b2ce03.png "continuous-delivery-pipeline-3fa"
[94]: https://aws.amazon.com#aws-element-modal-81c4bdf1-dd3b-44ce-add2-69e5157c0357
[95]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-4.8f2cb5ff3bf279f827bd783dae280c51a0c4ea7d.png "continuous-delivery-pipeline-4"
[96]: https://aws.amazon.com#aws-element-modal-9f0f304e-beb2-4af0-ac52-ea400f2c5f99
[97]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-4a.023bcbf4fddf4b0365a67a4b6595216c43035d30.png "continuous-delivery-pipeline-4a"
[98]: https://aws.amazon.com#aws-element-modal-c30a9ccd-3c64-4d16-a51d-2bb778ff55b7
[99]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-4a2.15c6b80a98479055414029291984339f57abe771.png "continuous-delivery-pipeline-4a2"
[100]: https://aws.amazon.com#aws-element-modal-a0d7bba1-4381-4835-ac4a-1784c46d5c72
[101]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-4b.5e29fe1a9cfd518a11bfedc0ba91e3fd6f09d0b4.png "continuous-delivery-pipeline-4b"
[102]: https://aws.amazon.com#aws-element-modal-ea88c8f9-5473-477a-a8d8-c5d6396c5c38
[103]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-6e.0d4a156c8a4e9dd945342b8aacd57b3723d9c46b.png "continuous-delivery-pipeline-6e"
[104]: https://aws.amazon.com#aws-element-modal-185b67e0-b2e7-4a6b-9e50-c7bfe755ff5d
[105]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-5b1.765c39f52f6f882ef3d96c86d31ca01cc6a4b830.png "continuous-delivery-pipeline-5b1"
[106]: https://aws.amazon.com#aws-element-modal-208888fa-454c-4700-ac51-a4166686f01d
[107]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-5c.e6e13229823beaf1d297129cc5633d09b28bce1c.png "continuous-delivery-pipeline-5c"
[108]: https://aws.amazon.com#aws-element-modal-7027c58d-c77b-4368-a521-1905b10b924d
[109]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-5c1.d34485eb0f5e71c674bfd733f5ee3c3813990a7a.png "continuous-delivery-pipeline-5c1"
[110]: https://aws.amazon.com#aws-element-modal-68ef6669-f167-49b0-a54e-88f55215b021
[111]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-6b2.5fbba91a8dc8f9576dbfd960073f86cf5a5386e8.png "continuous-delivery-pipeline-6b2"
[112]: https://aws.amazon.com#aws-element-modal-2ba9d18a-d511-4dbd-81d1-88ef64abc59a
[113]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-6c2.e43e0135ee6b671aaea0c0addec08926a02a7614.png "continuous-delivery-pipeline-6c2"
[114]: https://aws.amazon.com#aws-element-modal-ec541e12-c15a-4a78-8772-d8f27dd3c620
[115]: https://aws.amazon.com#aws-element-modal-9013414c-413b-42b0-b4c5-13ab74403ec6
[116]: https://aws.amazon.com#aws-element-modal-d6543d9b-2b57-408f-b438-f8f7ca06c331
[117]: https://aws.amazon.com#aws-element-modal-f5742de6-684c-4e11-bd50-ce87f0788bc2
[118]: https://aws.amazon.com#aws-element-modal-40b47117-b1d8-4446-9c2b-a5c758dc48bf
[119]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-5d-6.b847a3924ee9f5aa219fb06ea1f28e7d3208791e.png "continuous-delivery-pipeline-5d-6"
[120]: https://aws.amazon.com#aws-element-modal-766719be-432a-42c0-a713-ceacfbdb7b55
[121]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-6.13fe219f53089b3157bb041b18497f058b568a6e.png "continuous-delivery-pipeline-6"
[122]: https://aws.amazon.com#aws-element-modal-22f8a2b3-5ad8-4ad7-8571-805160572e40
[123]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-6d.b1d3fe5dc10c1c2430e8b074edc4d08db9fc1cea.png "continuous-delivery-pipeline-6d"
[124]: https://aws.amazon.com#aws-element-modal-2ac54dfb-6d32-4984-aeca-7de79ef959bc
[125]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-7a.d9e218fdf1894caf84ff2009de4bd9d767066549.png "continuous-delivery-pipeline-7a"
[126]: https://aws.amazon.com#aws-element-modal-fc6f8902-1482-442c-b56b-9724faaabc6b
[127]: https://d1.awsstatic.com/tmt/tmt_continuous-delivery-pipeline/continuous-delivery-pipeline-7b.3aea0395fd5cc60ec793b49339766564196f2da3.png "continuous-delivery-pipeline-7b"
[128]: https://aws.amazon.com#aws-element-modal-df31d29d-35d9-4df0-b946-752ab6c8f130
[129]: http://docs.aws.amazon.com/codepipeline/latest/userguide/getting-started-4.html
[130]: https://blogs.aws.amazon.com/application-management/post/Tx2CIB02ZO05ZII/Explore-Continuous-Delivery-in-AWS-with-the-Pipeline-Starter-Kit
[131]: https://aws.amazon.com/devops/continuous-delivery/
[132]: https://aws.amazon.com/getting-started/tutorials/continuous-deployment-pipeline/
