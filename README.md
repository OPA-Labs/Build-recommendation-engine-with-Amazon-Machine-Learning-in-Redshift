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
We upload data into S3. Then, we used Amazon Machine Learning for training model and prediction. All of the output will be stored into S3.

![1.jpg](/images/1.jpg)


## Prerequisites

>Make sure your are in US East (N. Virginia), which short name is us-east-1.

>Prepare the IAM credential.

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

### Create a Amazon Redshift Cluster

2.1.    On the service menu, click **Redshift**.

2.2.    Click **Launch Cluster**.

2.3.    In the cluster details part,
* For Cluster identifier, type **redshiftdemo**.
* For Database name, type **redshiftdemo**.
* For Database port, type **5439**.
* For Master User Name, type **root**.
* For Master User Password, type **Redshift123**.
* Confirm the password

2.4.    Click **Continue**.

2.5.    In Node configuration part,
* For Node Type, select **dc1.large**.
* For Cluster Type, select **Single Node**.
* For Number of Compute Nodes, type **1**.

2.6.    Click **Continue**.

2.7.    In Additional configuration part,
* For Choose a VPC, select **default VPC**.
* ForVPC Security Groups, choose **workshop-sg** which you created before.

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
