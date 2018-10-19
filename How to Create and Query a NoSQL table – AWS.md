
[Source](https://aws.amazon.com/getting-started/tutorials/create-nosql-table/ "Permalink to How to Create and Query a NoSQL table – AWS")

# How to Create and Query a NoSQL table – AWS

#  Create and Query a NoSQL Table with Amazon DynamoDB

In this tutorial, you will learn how to create a simple table, add data, scan and query the data, delete data, and delete the table by using the [DynamoDB console][45]. DynamoDB is a fully managed NoSQL database that supports both document and key-value store models. Its flexible data model, reliable performance, and automatic scaling of throughput capacity make it a great fit for mobile, web, gaming, ad tech, IoT, and many other applications.

Everything done in this tutorial is [free-tier][46] eligible.

#### Create and Query a NoSQL Table Requires an Account

[Create a Free Account in Minutes][47]

* * *

AWS Free Tier offers 25 GB of storage, up to 200 million requests per month with Amazon DynamoDB.  

[View AWS Free Tier Details »][48]  

 

Open the [AWS Management Console][43] so that you can keep this step-by-step guide open. When this screen loads, begin typing _DynamoDB _in the search bar and choose to open the DynamoDB console.

[

![AWS Console Image][49]

][50]

(choose to zoom)  

![tmt_create-nosql-table-01][51]

* * *

##  Step 1: Create a NoSQL Table

In this step, you will use the DynamoDB console to create a table.  

* * *

a. In the DynamoDB console, choose **Create table**.  

[

![create-select-nosql1][52]

][53]

(choose to zoom)  

![create-select-nosql1][52]

* * *

b. We will use a music library as our use case for this tutorial.  In the **Table name** box, type _Music._

[

![create-select-nosql_a0][54]

][55]

(choose to zoom)  

![create-select-nosql_a0][54]

* * *

c. The** **partition key is used to spread data across partitions for scalability. It's important to choose an attribute with a wide range of values and that is likely to have evenly distributed access patterns. Type _Artist_ in the _**Partition key**_ box.

 

[

![create-select-nosql_a1][56]

][57]

(choose to zoom)  

![create-select-nosql_a1][56]

* * *

d. Because each artist may write many songs, you can enable easy sorting with a sort key. Select the** Add sort key**_ _check box. Type so_ngTitle_ in the **Add sort key**_ _box.

 

[

![create-select-nosql_a2][58]

][59]

(choose to zoom)  

![create-select-nosql_a2][58]

* * *

e. Next, you will enable DynamoDB auto scaling for your table.

DynamoDB auto scaling will change the read and write capacity of your table based on request volume. Using an AWS Identity and Access Management (AWS IAM) role called _DynamoDBAutoscaleRole_, DynamoDB will manage the auto scaling process on your behalf. DynamoDB creates this role for you the first time you enable auto scaling in an account.

Instruct DynamoDB to create the role by clearing the **Use default settings **check box.  

[

![create-select-nosql23][60]

][61]

(choose to zoom)  

![create-select-nosql23][60]

* * *

f. Scroll down the screen past **Secondary indexes**, **Provisioned capacity**, and **Auto Scaling** to the **Create** button. We won't change these settings for the tutorial.

In the **Auto Scaling** section, notice that DynamoDB will create the _DynamoDBAutoscaleRole_ role for you.

Now  choose  **Create**.

When the **Music** table is ready to use, it appears in the table list with a  check box .

Congratulations! You have created a NoSQL table using the DynamoDB console.  

[

![create-select-nosql25][62]

][63]

(choose to zoom)  

![create-select-nosql25][62]

* * *

##  Step 2: Add Data to the NoSQL Table

In this step, you will add data to your new DynamoDB table.

* * *

a. Select the **Items **tab. On the **Items** tab, choose **Create  item **.

[

![create-select-nosql_a3][64]

][65]

(choose to zoom)

![create-select-nosql_a3][64]

* * *

b. In the data entry window, type the following:

* For the **Artist** attribute, type _No One You Know._
* For the ** songTitle ** attribute, type _Call Me Today._

Choose **Save** to save the item.

[

![create-select-nosql9][66]

][67]

(choose to zoom)  

![create-select-nosql9][66]

* * *

c. Repeat the process to add a few more items to your _Music_ table:

* **Artist**:_ No One You Know_; **songTitle**: _My Dog Spot_
* **Artist**: _No One You Know_; **songTitle**: _Somewhere Down The Road_
* **Artist**: _The Acme Band_; **songTitle**: _Still in Love_
* **Artist**: _The Acme Band_; **songTitle**: _Look Out, World_

[

![create-select-nosql_a5][68]

][69]

(choose to zoom)  

![create-select-nosql_a5][68]

* * *

##  Step 3: Query the NoSQL Table

In this step, you will search for data in the table using query operations. In DynamoDB, query operations are efficient and use keys to find data. Scan operations traverse the entire table.  

* * *

a. In the drop-down list in the dark gray banner above the items, change **Scan **to **Query**. 

[

![create-select-nosql24][70]

][71]

(choose to zoom)

![create-select-nosql24][70]

* * *

b. You can use the console to query the _Music_ table in various ways. For your first query, do the following:

* In the **Artist** box, type _No One You Know_, and choose **Start search**. All songs performed by _No One You Know_ are displayed.

Try another query:

* In the **Artist** box, type _The Acme Band_, and choose **Start search**. All songs performed by _The Acme Band_ are displayed.

[

![create-select-nosql12][72]

][73]

(choose to zoom)

![create-select-nosql12][72]

* * *

c. Try another query, but this time  narrow  down the search results:

* In the **Artist** box, type _The Acme Band_.
* In the **songTitle** box, select **Begins with** from the drop-down list and type _S_.
* Choose **Start search**.**  **Only "Still in Love" performed by _The Acme Band_ is displayed.

 

[

![create-select-nosql15][74]

][75]

(choose to zoom)

![create-select-nosql15][74]

* * *

##  Step 4: Delete an Existing Item

In this step, you will delete an item from your DynamoDB table.

* * *

a. Change the **Query **drop-down list back to **Scan**.  

Select the  check box  next to _The Acme Band_. In the **Actions **drop-down list, choose **Delete**. You will be asked whether to delete the item.  Choose  **Delete **and your item is deleted.

[

![create-select-nosql_a6][76]

][77]

(choose to zoom)

![create-select-nosql_a6][76]

* * *

##  Step 5: Delete a NoSQL Table

In this step, you will delete your DynamoDB table.

* * *

a. You can easily delete a table from the DynamoDB console. It is a best practice to delete tables you are no longer using so that you don't keep getting charged for them.

* In the DynamoDB console, choose the option next to the **Music **table and then choose **Delete table**.
* In the confirmation dialog box, choose **Delete**.

[

![create-select-nosql20][78]

][79]

(choose to zoom)

![create-select-nosql20][78]

#  Congratulations!

You have created your first DynamoDB table, added items to your table, and then queried the table to find the items you wanted. You also learned how to visually manage your DynamoDB tables and items through the AWS Management Console.

DynamoDB is a great fit for mobile, web, gaming, ad tech, and IoT applications where scalability, throughput, and reliable performance are key considerations.

 

##  Next Steps

Now that you have learned how to create, manage, and query tables and items from the AWS Management Console, you can progress to the next tutorial where you will learn how to import large amounts of data and easily find the information you need. You'll import a movie database to see how you can quickly find details about your favorite actors and characters.  

[See Getting Started with DynamoDB »][80]  

[45]: https://console.aws.amazon.com/console/home?region=us-east-1
[46]: http://aws.amazon.com/free/
[47]: https://portal.aws.amazon.com/gp/aws/developer/registration/index.html
[48]: https://aws.amazon.com/free/
[49]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/tmt_create-nosql-table-01.3ffc95e3ceaa164c88411a28881199be97bf15b7.png "AWS Console Image"
[50]: https://aws.amazon.com#aws-element-modal-64e1baa3-d734-4757-8647-2aca443ab806
[51]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/tmt_create-nosql-table-01.3ffc95e3ceaa164c88411a28881199be97bf15b7.png "tmt_create-nosql-table-01"
[52]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql1.632300f6c11874ff2d8285f424722d855b14fd7a.png "create-select-nosql1"
[53]: https://aws.amazon.com#aws-element-modal-aa745981-39d7-4ac5-9ef3-e5a2c1bc3f8f
[54]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a0.255040a90fe6e9244559633ca270867339d575b4.png "create-select-nosql_a0"
[55]: https://aws.amazon.com#aws-element-modal-1fc1166a-aaf6-45c4-875a-113b4378c07c
[56]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a1.3b2803a89e2924a5ec6a3ac4cff7f2fa136c2788.png "create-select-nosql_a1"
[57]: https://aws.amazon.com#aws-element-modal-92a9e54f-31a8-4347-bbd7-8abd2942b769
[58]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a2.277ca24c69c436ebb963f93214d437a06f52688d.png "create-select-nosql_a2"
[59]: https://aws.amazon.com#aws-element-modal-54a267fd-1499-407b-9374-84dff010b775
[60]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql23.34a2fa39c751f16935711006e1f5b1d225cf5164.png "create-select-nosql23"
[61]: https://aws.amazon.com#aws-element-modal-c8482463-af6b-4362-b3cd-f9b6f5595bd2
[62]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql25.a044737a6d36e7576e2f3a4520cddf2a270e70c9.png "create-select-nosql25"
[63]: https://aws.amazon.com#aws-element-modal-6e25fc84-d53c-4aa7-b0f2-b75eadc5d46b
[64]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a3.50fe14db3830fb8121c5332c3d37972306e361aa.png "create-select-nosql_a3"
[65]: https://aws.amazon.com#aws-element-modal-2f9543d0-3d2b-40bf-8e86-536164e76b1b
[66]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql9.b78ead5b6daf46a3bbd6987320d406f6b5711ffc.png "create-select-nosql9"
[67]: https://aws.amazon.com#aws-element-modal-7113e06f-d6ea-4d3c-9822-2ef67aa2fc97
[68]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a5.52dfaa8216df4c3225de5160e69fb183837517ef.png "create-select-nosql_a5"
[69]: https://aws.amazon.com#aws-element-modal-c075d9f6-7439-4ed2-8516-fea5f307e230
[70]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql24.79fb29928f05e319301a3a61ccbfad7d45ee5286.png "create-select-nosql24"
[71]: https://aws.amazon.com#aws-element-modal-453d656e-8280-4ecf-bc63-b4ec72fa4d30
[72]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql12.283eeef547e86b145957e877359dfb50389ce7f9.png "create-select-nosql12"
[73]: https://aws.amazon.com#aws-element-modal-90644d43-1b70-40bd-8ae6-3abb5ecf6222
[74]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql15.2c6bc1a737f7f556a2c81ae9a4b7e19b635ba76d.png "create-select-nosql15"
[75]: https://aws.amazon.com#aws-element-modal-c1fe93fa-e87d-4f42-aafd-180110e27110
[76]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql_a6.a86331a060468798b27e3a01f0bd9e0f0a16006b.png "create-select-nosql_a6"
[77]: https://aws.amazon.com#aws-element-modal-10c1a055-6fd8-4c84-b680-13c397c5968f
[78]: https://d1.awsstatic.com/tmt/tmt_create-query-nosql/create-select-nosql20.1ad84f7ac12d341c21f4a1790b95a55a5290c6e0.png "create-select-nosql20"
[79]: https://aws.amazon.com#aws-element-modal-ef048eb3-46ef-4a3d-ab52-66025e6c95d7
[80]: http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/GettingStarted.JsShell.html
[81]: https://aws.amazon.com/getting-started/tutorials/create-nosql-table/
