Build recommendation engine with Amazon Machine Learning in Redshift
================================================================

## Introduction
### Overview
In this lab, you will build a smart solution using [Amazon Redshift](https://aws.amazon.com/redshift/) and [Amazon Machine Learning](https://aws.amazon.com/machine-learning/) that predict rental bikes for Capital bikeshare system.

## About this lab
### Scenario

The dataset contains daily amount of rental bikes between years 2011 and 2012 in Capital bikeshare system with the corresponding weather and seasonal information.
All we want to know is how much bikes we should prepare for the next week. To avoid the situation when the supply could not meet the demand.

### Architecture Diagram
We used EC2 for import data into Redshift. Then, we used Amazon Machine Learning for training model and prediction. All of the output will be stored into S3.

![1.png](/images/1.png)


## Prerequisites

>Make sure your are in US East (N. Virginia), which short name is us-east-1.

>Prepare the IAM credential.

>Download the file **day_part_two.csv**.

## Lab tutorial
### Create a Security Group

1.1. 	On the service menu, click **EC2**.

1.2. 	On the left panel, click **Security Group**, and click **Create Security Group**.

1.3.    For Security group name, type **workshop-sg**.

1.4.    For Description, type **workshop-sg**.

1.5.    For VPC, choose **default**.

1.6.    For Inbound, click **Add Rule**.

1.7.    For Inbound type, choose **All traffic**.

1.8.    For Inbound Source, choose **Anywhere**.

1.9.    Click **Create**.

![2.png](/images/2.png)

### Create a Amazon Redshift Cluster

2.1.    On the service menu, click **Redshift**.

2.2.    Click **Launch Cluster**.

2.3.    In the Cluster details part,
* For Cluster identifier, type **redshiftdemo**.
* For Database name, type **redshiftdemo**.
* For Database port, type **5439**.
* For Master User Name, type **root**.
* For Master User Password, type **Redshift123**.
* Confirm the password.

>Copy the Cluster identifier, Database name, User Master Name and Password in text editor, we will use later.

2.4.    Click **Continue**.

2.5.    In Node configuration part,
* For Node Type, select **dc1.large**.
* For Cluster Type, select **Single Node**.
* For Number of Compute Nodes, type **1**.

2.6.    Click **Continue**.

2.7.    In Additional configuration part,
* For Choose a VPC, select **default VPC**.
* For VPC Security Groups, choose **workshop-sg** which you created before.

2.8.    Click **Continue**.

2.9.    In Review part,
* Click **Launch Cluster**.

2.10.   For now, you have created a Redshift cluster, continue the following guide, you will know how to establish a connection with Redshift cluster.

### Setup EC2 environment for SQL WorkbenchJ

3.1.    On the service menu, click **EC2**.

3.2.    On the left panel, click **AMIs**.

3.3.    For the filter, choose **Public images**.

3.4.    For the filter, type **ami-f4ca93e3**, then click **search**.

![2.jpg](/images/2.jpg)

3.5.    Choose images, then click **Launch**.

3.6.    For the instance type, choose **t2.small**.

3.7.    Click **Next: Configure Instance Detail**.

3.8.    For network, keep the setting as default.

3.9.    Click **Next: Add storage**.

3.10.   For storage, keep the setting as default.

3.11.   Click **Next: Tag Instance**.

3.12.   For tag instance, type values as **Workshop**.

3.13.   Click **Next: Configure Security Group**.

3.14.   For configure Security Group, **Select an existing security group: workshop-sg**.

3.15.   Click **Review and Launch**.

3.16.   For selecting an existing key pair part, select **proceed without key**.

3.17.   Select yes about **I acknowledge that I have access to the selected private key file, and that without this file, I wont't be able to log into my instance**.

3.18.   Click **Launch**.

### Connect to your instance (RDP Connect for Linux and Mac Users)

>if you are running Mac OS X, you can download the CoRD RDS client: http://cord.sourceforge.net/

4.1.    Open your RDP client, for server, paste in the Public DNS of EC2 in the blank.

4.2.    Click **Connect**.

4.3.    For Username, type **Administrator**.

4.4.    For Password, type **5m@;xE3c9iB**.

>After you login into EC2 which generated from AMI, we can use SQL Workbench as a connection tool in EC2. Continue the lab you will learn how to use SQL Workbench to connect to Redshift and import data into Redshift.

### Connect to your instance (RDP Connect for Windows Users)

5.1.    Open your RDP client, for server, paste in the Public DNS of EC2 in the blank.

5.2.    Click **Connect**.

5.3.    For Username, type **Administrator**.

5.4.    For Password, type **5m@;xE3c9iB**.

5.5.    Click **OK**.

5.6.    Click Yes to connect when you may see a security connection warning.

>After you login into EC2 which generated from AMI, we can use SQL Workbench as a connection tool in EC2. Continue the lab you will learn how to use SQL Workbench to connect to Redshift and import data into Redshift.

### Upload a file to S3 bucket
>Before connect with SQL Workbench, we need to upload dataset into S3 bucket. Please following the guide to continue the lab.

6.1. On the service menu, click **S3**, Click **Create Bucket**.

6.2. For Bucket Name, type **Unique Name**.

6.3. For Region, choose **US East (N. Virginia)**, Click **Create**.

6.4. Select the bucket which you created before, Click **Upload**, Click **Add files**.

6.5. Select the **day_part_two.csv** file which in the Github share folder, then choose Click **Start Upload**.

### Establish a connection with Redshift cluster through SQL Workbench

7.1.    Click SQLWorkbenchJ which in the **workshop folder**.

7.2.    About **select connect profile**, for URL, type **jdbc:redshift://REDSHIFT ENDPOINT:5439/redshiftdemo**.

7.3.    For Username, type the username which you created in the AWS console.

7.4.    For Password, type the username which you created in the AWS console.

7.5.    Select **AutoCommit**.

7.6.    Click **OK**.

>Congrats! You have established a connection with Redshift cluster. Continue the following guide, you will create a table and import reference data into Redshift.

### Create a table in Redshift via SQLWorkbench

8.1.    Create a table in Redshift, type the following commend into SQLWorkbench's statement.



### Import data in Redshift

9.1.    Import bike data in Redshift's table, type the following commend into SQLWorkbench's statement.

    copy bike

>Please noticed that bucket name should be copy from your own AWS account's S3 bucket. For example my bucket is **aws-ecv-seminar**.

9.2.    After submit the SQL commend, you will see **Warnings: Load into table 'bike' completed. 731 record(s) loaded successfully. 0 rows affected. COPY executed successfully' in SQL Workbench's message.

> Congrats! You have completed the Redshift part as a data source. Let's move to next section to learn how to use Amazon Machine Learning.

### Create Model via Amazon Machine Learning

10.1. 	On the service menu, click **Machine Learning**.

10.2. 	Click ‘Get Started’ and **Launch**.

10.3. 	For **where is your data**, choose **Redshift**.

10.4.   For Cluster identifier, choose the redshift which you created.

10.5.   For Database name, type database name which you created.

10.6.   For Database username, type username which you created.

10.7.   For Database password, type password which you created.

10.8.   For IAM role, select **Test Access**.

> You will see **IAM role created. Amazon ML can now access Amazon Redshift**.

10.9.   For SQL Query, type

    select * from bike

10.10.  For Amazon S3 staging location, type **aws-ecv-seminar/output**.

> Find the bucket which you created before, then create a folder which named output.

10.11. 	For Datasource name, type **aml‐ver1**, Click **Verify**.

> Note: You will see ‘The validation is successful. To go to the next step, choose Continue’

10.12. 	Click **Continue**.

10.13. 	In Schema part:

* About first line in the column name, click **yes** when you see the question: Does the first line in your CSV contain the column names?
* For	Datatype,	choose	season/mnth/weekday/workingday/weathersit	as **Catagorical**
* For Datatype, choose cnt as **Numetric**
* Click ‘Continue’

10.14. 	In Target part:

* For target, choose **cnt** as target for prediction
* Click **Continue**

10.15. In Row ID part:

* Click **Review**

10.16. In Review part:

* Click **Finish**

10.17. In ML model settings part:

* Click **Review**

10.18. In Review part:

* Click **Create ML Model**
* For this moment, you will see the message said **status: Pending**, you can test this machine learning until the status go to **completed**.

### Testing with Amazon Machine Learning

11.1. For Dashboard, click **ML‐model** which AML created. 

11.2. For the left panel, click **Try real-time predictions**

* For season, you can type 1 to 4
* For mnth, you can type 1 to 12
* For weekday, you can type 1 to 7
* For workingday, you can type 1 to 2
* For weathersit, you can type 1 to 4

11.3. Then, click **create prediction**, you will see the predict value in the right panel. 	


## Conclusion
Congratulations! You now have learned how to:
* Create a Redshift cluster, build Redshift table and load data from Amazon S3.
* Create a Machine Learning Model
* Train the Machine Learning Model, using historic data about rental bikes.
* Predict the rental amount for the future sharebike system with S3 and Amazon Machine Learning
