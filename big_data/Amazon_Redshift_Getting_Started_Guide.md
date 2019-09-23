# Amazon Redshift Getting Started Guide

Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud. An Amazon Redshift data warehouse is a collection of
computing resources called nodes, which are organized into a group called a cluster. Each cluster runs an
Amazon Redshift engine and contains one or more databases.


[TOC]


# [Step 1: Set Up Prerequisites](#step-1-set-up-prerequisites)

Before you begin setting up an Amazon Redshift cluster, make sure that you complete the following prerequisites in this section:

*   [Sign Up for AWS](#rs-gsg-prereq-signup)
*   [Install SQL Workbench]()

## [Sign Up for AWS Educate](#sign-up-for-aws)

If you don’t already have an AWS account, you must sign up for one. 

1. Open [https://aws.amazon.com/education/awseducate/](https://aws.amazon.com/education/awseducate/).

2. Follow the online instructions.

   **Important**

   Do not create AWS Educate Starter account because it does not support Redshift.

Then you can log in [AWS Management Console](https://console.aws.amazon.com/)

## [Install SQL Workbench](#install-sql-workbench)
SQL workbench is a GUI client to connect SQL databases. We will use it to connect AWS redshift database (PostgreSQL). You can install it following [To Install SQL Workbench/J on Your Client Computer](#to-install-sql-workbenchj-on-your-client-computer)  or just download it from [sql workbench via google drive](https://drive.google.com/file/d/1B-s_MwWwDXfIxECOyIuaKv4v9_2oP6Ld/view?usp=sharing)

* Download it and uncompress it, enter the folder.

* Uncompress OpenJDK for your OS locally.

  ![openjdk](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/openjdk.png)

* Open SQL Workbench by double clicking `sqlworkbench.cmd` for Windows or run ` sh sqlworkbench.sh` in terminal for Macos/Linux. 


# [Step 2: Create an IAM Role](#step-2-create-an-iam-role)

For any operation that accesses data on another AWS resource, your cluster needs permission to access the resource and the data on the resource on your behalf. An example is using a COPY command to load data from Amazon S3. You provide those permissions by using AWS Identity and Access Management (IAM). You do so either through an IAM role that is attached to your cluster or by providing the AWS access key for an IAM user that has the necessary permissions.

To best protect your sensitive data and safeguard your AWS access credentials, we recommend creating an IAM role and attaching it to your cluster. For more information about providing access permissions, see [Permissions to Access Other AWS Resources](https://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-access-permissions.html).

In this step, you create a new IAM role that enables Amazon Redshift to load data from Amazon S3 buckets. In the next step, you attach the role to your cluster.

## [Create an IAM Role for Amazon Redshift](#to-create-an-iam-role-for--amazon-redshift)

1.  Sign in to the AWS Management Console and open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).
    
    ![2019-08-30_18-15](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-15.png)
    
2. In the navigation pane, choose **Roles**.

3. Choose **Create role**.

4. In the **AWS Service** group, choose **Redshift.**

5. Under **Select your use case**, choose **Redshift - Customizable** then choose **Next: Permissions**.

   ![2019-08-30_18-07](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-07.png)

6. On the **Attach permissions policies** page, choose **AmazonS3ReadOnlyAccess**. You can leave the default setting for **Set permissions boundary**. Then choose **Next: Tags**.

   ![2019-08-30_18-08](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-08.png)

7. The **Add tags** page appears. You can optionally add tags. Choose **Next: Review**.

8. For **Role name**, type a name for your role. For this tutorial, type `myRedshiftRole`.

9. Review the information, and then choose **Create Role**.

10. Choose the role name of the role you just created.

11. Copy the **Role ARN** to your clipboard—this value is the Amazon Resource Name (ARN) for the role that you just created. You use that value when you use the COPY command to load data in [Step 6: Load Sample Data from Amazon S3](#step-6-load-sample-data-from-amazon-s3).


Now that you have created the new role, your next step is to attach it to your cluster. You can attach the role when you launch a new cluster or you can attach it to an existing cluster. In the next step, you'll attach the role to a new cluster.


# [Step 3: Launch a Sample Amazon Redshift Cluster](#step-3-launch-a-sample-amazon-redshift-cluster)

Now that you have the prerequisites completed, you can launch your Amazon Redshift cluster.

**Important** 
_The cluster that you are about to launch is live (and not running in a sandbox). You incur the standard Amazon Redshift usage fees for the cluster until you delete it. If you complete the tutorial described here in one sitting and delete the cluster when you are finished, the total charges are minimal._

## [To Launch an Amazon Redshift Cluster](#to-launch-an-amazon-redshift-cluster)

1.  Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console.aws.amazon.com/redshift/](https://console.aws.amazon.com/redshift/).  

2. In the main menu, select the region in which you want to create the cluster. For the purposes of this tutorial, select **US West (Oregon)**. 

   ![1567136734-4d235c5ca5e53a7402806a9379ecf956](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567136734-4d235c5ca5e53a7402806a9379ecf956.png)

3. On the Amazon Redshift Dashboard, choose **Quick launch cluster**.

   The Amazon Redshift Dashboard looks similar to the following.  

   ![1567136734-36214042904b6cfa0628a5ded9e24527](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567136734-36214042904b6cfa0628a5ded9e24527.png)

4. On the Cluster specifications page, enter the following values and then choose **Launch cluster**:

   *   **Node type**: Choose **dc2.large**.
   *   **Number of compute nodes**: Keep the default value of **2**.
   *   **Cluster identifier**: Enter the value **redshift-cluster-NetID** (You must include your NetID for homework submission).
   *   **Master user name**: Keep the default value of **awsuser**.
   *   **Master user password** and **Confirm password**: Enter a password for the master user account.
   *   **Database port**: Accept the default value of **5439**.
   *   **Available IAM roles**: Choose **myRedshiftRole**.

   Quick Launch automatically creates a default database named **dev**.  

   ![2019-08-30_18-28](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-28.png)

**Note** 
_Quick Launch uses the default virtual private cloud (VPC) for your region. If a default VPC doesn't exist, Quick Launch returns an error. If you don't have a default VPC, you can use the standard Launch Cluster wizard to use a different VPC. For more information, see [Creating a Cluster by Using Launch Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)._

5. A confirmation page appears and the cluster takes a few minutes to finish. Choose **Close** to return to the list of clusters. 

   ![2019-08-30_18-33](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-33.png)

   ![2019-08-30_18-34](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-34.png)

6. On the Clusters page, choose the cluster that you just launched and review the **Cluster Status** information. Make sure that the **Cluster Status** is **available** and the **Database Health** is **healthy** before you try to connect to the database later in this tutorial. 

   ![2019-08-30_18-37](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-37.png)

7. On the Clusters page, choose the cluster that you just launched, choose the **Cluster** button, then **Modify cluster**. Choose the **VPC security groups** to associate with this cluster, then choose **Modify** to make the association. Make sure that the **Cluster Properties** displays the **VPC security groups** you chose before continuing to the next step.  

   ![2019-08-30_18-40](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-40.png)



# [Step 4: Authorize Access to the Cluster](#step-4-authorize-access-to-the-cluster)

In the previous step, you launched your Amazon Redshift cluster. Before you can connect to the cluster, you need to configure a security group to authorize access. If you launched your cluster in the EC2-VPC platform, follow the steps in [To Configure the VPC Security Group (EC2-VPC Platform)](#rs-gsg-how-to-authorize-access-vpc-security-group).

## [To Configure the VPC Security Group ](#to-configure-the-vpc-security-group-ec2-vpc-platform)

1.  In the Amazon Redshift console, in the navigation pane, choose **Clusters**.
    
2.  Choose `redshift-cluster-<NetID>` to open it, and make sure that you are on the **Configuration** tab.
    
3.  Under **Cluster Properties**, for **VPC Security Groups**, choose your security group. 
    
    ![2019-08-30_18-46](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-46.png)
    
4. After your security group opens in the Amazon EC2 console, choose the **Inbound** tab. 

   ![1567138244-712aaed0b855d6868312c365e20c734d](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138244-712aaed0b855d6868312c365e20c734d.png)

5. Choose **Edit**, **Add Rule**, and enter the following, then choose **Save**:

   * **redshift-cluster-NetID** 

   * **Type: Custom TCP Rule**.

   * **Protocol**: **TCP**.

   * **Port Range**: type the same port number that you used when you launched the cluster. The default port for Amazon Redshift is `5439`, but your port might be different.
   
   *   **Source**: select **Custom**, then type `0.0.0.0/0`. 
       
       **Important**  
       
       Using 0.0.0.0/0 is not recommended for anything other than demonstration purposes because it allows access from any computer on the internet. In a real environment, you would create inbound rules based on your own network settings. 
       
       ![1567138244-edbbb26cf2a3effcd6e22d32537240dc](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138244-edbbb26cf2a3effcd6e22d32537240dc.png)



# [Step 5: Connect to the Sample Cluster and Run Queries](#step-5-connect-to-the-sample-cluster-and-run-queries)

To query databases hosted by your Amazon Redshift cluster, you have two options:

*   Connect to your cluster and run queries on the AWS Management Console with the Query Editor.
    
    If you use the Query Editor, you don't have to download and set up a SQL client application.
    
*   Connect to your cluster through a SQL client tool, such as SQL Workbench/J.
    

**Topics**

*   [Querying a Database Using the Query Editor](#gsg-query-editor)
*   [Querying a Database Using a SQL Client](#connect-using-sql-client)

## [Querying a Database Using the Query Editor](#querying-a-database-using-the-query-editor)

Using the Query Editor is the easiest way to run queries on databases hosted by your Amazon Redshift cluster. After creating your cluster, you can immediately run queries by using the Query Editor on the Amazon Redshift console.

The following cluster node types support the Query Editor:

*   DC1.8xlarge
*   DC2.large
*   DC2.8xlarge
*   DS2.8xlarge

Using the Query Editor, you can do the following:

*   Run single SQL statement queries.
*   Download result sets as large as 100 MB to a comma-separated value (CSV) file.
*   Save queries for reuse. You can't save queries in the EU (Paris) Region or the Asia Pacific (Osaka-Local) Region.
*   View query execution details for user-defined tables.

### [Query Editor Considerations](#query-editor-considerations)

For details about considerations when using the Query Editor, see [Querying a Database Using the Query Editor](https://docs.aws.amazon.com/redshift/latest/mgmt/query-editor.html) in the Amazon Redshift Cluster Management Guide.

### [Enabling Access to the Query Editor](#enabling-access-to-the-query-editor)

To access the Query Editor, you need permission. To enable access, attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies for AWS Identity and Access Management (IAM) to the AWS IAM user that you use to access your cluster.

If you have already created an IAM user to access Amazon Redshift, you can attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies to that user. If you haven't created an IAM user yet, create one and attach the policies to the IAM user.

**To attach the required IAM policies for the Query Editor**

1.  Sign in to the AWS Management Console and open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).
    
2.  Choose **Users**.
    
3.  Choose **Add user** or Choose the user that needs access to the Query Editor.
    
    ![2019-08-30_18-52](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-52.png)
    
    ![2019-08-30_18-53](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-53.png)
    
    
    
4. Choose **Add permissions**.

5. Choose **Attach existing policies directly**.

6. For **Policy names**, choose **AmazonRedshiftQueryEditor** and **AmazonRedshiftReadOnlyAccess**.

   ![2019-08-30_18-56](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-56.png)

7. Choose **Next: Review**.

8. Choose **Create user** or **Add permissions**.

   ![2019-08-30_18-57](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-57.png)

9. Save user **Access key ID** and **Secret access key**

   

   ![2019-08-30_18-58](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_18-58.png)

### [Using the Query Editor](#using-the-query-editor)

In the following example, you use the Query Editor to perform the following tasks:

*   Run SQL commands.
*   View query execution details.
*   Save a query.
*   Download a query result set.

**To use the Query Editor**

1.  Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console.aws.amazon.com/redshift/](https://console.aws.amazon.com/redshift/).
    
2.  In the navigation pane, choose **Query Editor**.
    
3.  In the **Credentials** dialog box, enter the following values and then choose **Connect**:
    
    * **Cluster**: Choose **redshift-cluster-NetID**.
    
    * **Database**: **dev**.
    
    * **Database user**: **awsuser**
    
    * **Password**: Enter the password that you specified when you launched the cluster.
    
      ![2019-08-30_19-06](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-06.png)
    
4.  For **Schema**, choose \*\*public \*\*to create a new table based on that schema. 
    
    ![2019-08-30_19-07](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-07.png)
    
5. Enter the following in the Query Editor window and choose **Run query** to create a new table.

   ```sql
   create table shoes(
   shoetype varchar (10),
   color varchar(10));
   ```

6. Choose **Clear**.

7. Enter the following command in the Query Editor window and choose **Run query** to add rows to the table.

   ```sql
   insert into shoes values 
   ('loafers', 'brown'),
   ('sandals', 'black');
   ```

8. Choose **Clear**.

9. Enter the following command in the Query Editor window and choose **Run query** to query the new table.

   ```sql
   select * from shoes;
   ```

   You should see the following results. 

   ![2019-08-30_19-10](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-10.png)

## [Querying a Database Using a SQL Client](#querying-a-database-using-a-sql-client)

Next, you connect to your cluster by using a SQL client tool and run a simple query to test the connection. You can use most SQL client tools that are compatible with PostgreSQL. For this tutorial, you use the SQL Workbench/J client. Complete this section by performing the following steps:

*   [Install SQL Client Drivers and Tools](#rs-gsg-sql-client)
*   [To Get Your Connection String](#rs-gsg-how-to-get-connection-string)
*   [To Connect from SQL Workbench/J to Your Cluster](#rs-gsg-how-to-connect-from-workbench)

After you complete this step, you can determine whether you want to load sample data from Amazon S3 in [Step 6: Load Sample Data from Amazon S3](https://github.com/awsdocs/amazon-redshift-getting-started-guide/blob/master/doc_source/rs-gsg-create-sample-db.md) or find more information about Amazon Redshift and reset your environment at [Where Do I Go From Here?](https://github.com/awsdocs/amazon-redshift-getting-started-guide/blob/master/doc_source/rs-gsg-clean-up-tasks.md#rs-gsg-where-do-i-go).

### [Install SQL Client Drivers and Tools](#install-sql-client-drivers-and-tools)

You can use most SQL client tools with Amazon Redshift JDBC or ODBC drivers to connect to an Amazon Redshift cluster. In this tutorial, you connect using SQL Workbench/J, a free, DBMS-independent, cross-platform SQL query tool. If you plan to use SQL Workbench/J to complete this tutorial, use the steps following to set up the Amazon Redshift JDBC driver and SQL Workbench/J. For more complete instructions for installing SQL Workbench/J, go to [Setting Up the SQL Workbench/J Client](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-using-workbench.html) in the _Amazon Redshift Cluster Management Guide_. If you use an Amazon EC2 instance as your client computer, install SQL Workbench/J and the required drivers on the instance.

**Note**
Install any third-party database tools that you want to use with your clusters yourself. Amazon Redshift doesn't provide or install any third-party tools or libraries.

#### [To Install SQL Workbench/J on Your Client Computer](#to-install-sql-workbenchj-on-your-client-computer)

1.  Review the [SQL Workbench/J software license](http://www.sql-workbench.net/manual/license.html#license-restrictions).
    
2.  Go to the [SQL Workbench/J website](http://www.sql-workbench.net/) and download the appropriate package for your operating system.
    
3.  Go to the [Installing and starting SQL Workbench/J page](http://www.sql-workbench.net/manual/install.html) and install SQL Workbench/J. 

**Important**
Note the Java runtime version prerequisites for SQL Workbench/J and ensure you are using that version. Otherwise, the client application doesn't run.
    

4.  Go to [Configure a JDBC Connection](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html) and download an Amazon Redshift JDBC driver to enable SQL Workbench/J to connect to your cluster.
    

For more information about using the Amazon Redshift JDBC or ODBC drivers, see [Configuring Connections in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/configuring-connections.html).

**If you have difficulties to install SQL Workbench, you can download it from [sql workbench via google drive](https://drive.google.com/file/d/1B-s_MwWwDXfIxECOyIuaKv4v9_2oP6Ld/view?usp=sharing)**

* Download it and uncompress it, enter the folder.

* Uncompress OpenJDK for your OS locally.

  ![openjdk](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/openjdk.png)

* Open SQL Workbench by double clicking `sqlworkbench.cmd` for Windows or run ` sh sqlworkbench.sh` in terminal for Macos/Linux.

### [To Get Your Connection String](#to-get-your-connection-string)

1.  In the Amazon Redshift console, in the navigation pane, choose **Clusters**.
    
2.  Choose `redshift-cluster-<NetID>` to open it, and make sure that you are on the **Configuration** tab.
    
3.  On the **Configuration** tab, under **Cluster Database Properties**, copy the JDBC URL of the cluster. **Note**
    The endpoint for your cluster is not available until the cluster is created and in the available state. 
    
    ![2019-08-30_19-17](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-17.png)

### [To Connect from SQL Workbench/J to Your Cluster](#to-connect-from-sql-workbenchj-to-your-cluster)

This step assumes you installed SQL Workbench/J.

1.  Open SQL Workbench/J.
    
2. Choose **File**, and then choose **Manage Drivers**. The **Manage Drivers** dialog box opens.

   ![2019-08-30_19-21_1](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-21_1.png)

   ![2019-08-30_19-22](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-22.png)

   ![2019-08-30_19-29](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-29.png)

   If the **Please select one driver** dialog box displays, choose **com.amazon.redshift.jdbc4.Driver** or **com.amazon.redshift.jdbc41.Driver** and then choose **OK**. SQL Workbench/J automatically completes the **Classname** box. Keep **Sample URL** blank, and choose **OK**.

3. Choose **File**, and then choose **Connect window**.

4. For **New profile**, enter a name for the profile.

5. For **Driver**, choose the driver that you just added.

6. For **URL**, copy the JDBC URL from the Amazon Redshift console and paste it here.

7. For **Username**, enter **awsuser** for the master user.

8. For **Password**, enter the password associated with the master user account.

9. Choose **Autocommit**.

10. Choose the **Save profile list** icon, as shown following. 

11. Choose **OK**.  

    ![2019-08-30_19-37](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-37.png)

12. Enter the following command in the query window and choose **SQL**, **Execute Current** to add rows to the table.

    ```sql
    create table shoes(
    shoetype varchar (10),
    color varchar(10));
    ```

13. Run the following command to add rows to the table.

    ```sql
    insert into shoes values 
    ('loafers', 'brown'),
    ('sandals', 'black');
    ```

14. Run the following command to query the new table.

    ```sql
    select * from shoes;
    ```


# [Step 6: Load Sample Data from Amazon S3](#step-6-load-sample-data-from-amazon-s3)

At this point, you have a database called `dev` and you are connected to it. Next, you create some tables in the database, upload data to the tables, and try a query. For your convenience, the sample data you load is available in an Amazon S3 bucket.

**Note**  

If you're using a SQL client tool, ensure that your SQL client is connected to the cluster.

After you complete this step, you can find more information about Amazon Redshift and reset your environment at [Where Do I Go From Here?](https://github.com/awsdocs/amazon-redshift-getting-started-guide/blob/master/doc_source/rs-gsg-clean-up-tasks.md#rs-gsg-where-do-i-go).

**To load sample data**

1.  Create tables.
    
    Individually copy and run the following create table statements to create tables in the `dev` database. For more information about the syntax, see [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) in the _Amazon Redshift Database Developer Guide_.
    
    ```sql
    create table users(
        userid integer not null distkey sortkey,
        username char(8),
        firstname varchar(30),
        lastname varchar(30),
        city varchar(30),
        state char(2),
        email varchar(100),
        phone char(14),
        likesports boolean,
        liketheatre boolean,
        likeconcerts boolean,
        likejazz boolean,
        likeclassical boolean,
        likeopera boolean,
        likerock boolean,
        likevegas boolean,
        likebroadway boolean,
        likemusicals boolean);
    
    create table venue(
        venueid smallint not null distkey sortkey,
        venuename varchar(100),
        venuecity varchar(30),
        venuestate char(2),
        venueseats integer);
    
    create table category(
        catid smallint not null distkey sortkey,
        catgroup varchar(10),
        catname varchar(10),
        catdesc varchar(50));
    
    create table date(
        dateid smallint not null distkey sortkey,
        caldate date not null,
        day character(3) not null,
        week smallint not null,
        month character(5) not null,
        qtr character(5) not null,
        year smallint not null,
        holiday boolean default('N'));
    
    create table event(
        eventid integer not null distkey,
        venueid smallint not null,
        catid smallint not null,
        dateid smallint not null sortkey,
        eventname varchar(200),
        starttime timestamp);
    
    create table listing(
        listid integer not null distkey,
        sellerid integer not null,
        eventid integer not null,
        dateid smallint not null  sortkey,
        numtickets smallint not null,
        priceperticket decimal(8,2),
        totalprice decimal(8,2),
        listtime timestamp);
    
    create table sales(
        salesid integer not null,
        listid integer not null distkey,
        sellerid integer not null,
        buyerid integer not null,
        eventid integer not null,
        dateid smallint not null sortkey,
        qtysold smallint not null,
        pricepaid decimal(8,2),
        commission decimal(8,2),
        saletime timestamp);
    ```
    
    ![2019-08-30_19-41](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-41.png)
    
2. Load sample data from Amazon S3 by using the COPY command. 

**Note**
​_We recommend using the COPY command to load large datasets into Amazon Redshift from Amazon S3              or DynamoDB. For more information about COPY syntax, see [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the _Amazon Redshift Database Developer Guide._
​    

The sample data for this tutorial is provided in an Amazon S3 bucket that is owned by Amazon Redshift. The bucket permissions are configured to allow all authenticated AWS users read access to the sample data files.

To load the sample data, you must provide authentication for your cluster to access Amazon S3 on your behalf. You can provide either role-based authentication or key-based authentication. We recommend using role-based authentication. For more information about both types of authentication, see [CREDENTIALS](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-credentials.html) in the Amazon Redshift Database Developer Guide.

For this step, you provide authentication by referencing the IAM role that you created and then attached to your cluster in previous steps. 

**Note**
_If you don't have proper permissions to access Amazon S3, you receive the following error message when running the COPY command: `S3ServiceException: Access Denied`._

The COPY commands include a placeholder for the Amazon Resource Name (ARN) for the IAM role, as shown in the following example.

```sql
copy users from 's3://awssampledbuswest2/tickit/allusers_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';
```

To authorize access using an IAM role, replace `<iam-role-arn>` in the CREDENTIALS parameter string with the role ARN for the IAM role that you created in [Step 2: Create an IAM Role](#step-2-create-an-iam-role).

Your COPY command looks similar to the following example\. 


```sql
copy users from 's3://awssampledbuswest2/tickit/allusers_pipe.txt' 
credentials 'aws_iam_role=arn:aws:iam::123456789012:role/myRedshiftRole' 
delimiter '|' region 'us-west-2';
```

To load the sample data, replace _<iam-role-arn>_ in the following COPY commands with your role ARN. Then run the commands individually in your SQL client tool.

```sql
copy users from 's3://awssampledbuswest2/tickit/allusers_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';

copy venue from 's3://awssampledbuswest2/tickit/venue_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';

copy category from 's3://awssampledbuswest2/tickit/category_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';

copy date from 's3://awssampledbuswest2/tickit/date2008_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';

copy event from 's3://awssampledbuswest2/tickit/allevents_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' timeformat 'YYYY-MM-DD HH:MI:SS' region 'us-west-2';

copy listing from 's3://awssampledbuswest2/tickit/listings_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';

copy sales from 's3://awssampledbuswest2/tickit/sales_tab.txt'
credentials 'aws_iam_role=<iam-role-arn>'
delimiter '\t' timeformat 'MM/DD/YYYY HH:MI:SS' region 'us-west-2';
```

1.  Now try the example queries. For more information, see [SELECT](https://docs.aws.amazon.com/redshift/latest/dg/r_SELECT_synopsis.html) in the _Amazon Redshift Developer Guide_.
    
    ```sql
    -- Get definition for the sales table.
    SELECT *    
    FROM pg_table_def    
    WHERE tablename = 'sales';    
    
    -- Find total sales on a given calendar date.
    SELECT sum(qtysold) 
    FROM   sales, date 
    WHERE  sales.dateid = date.dateid 
    AND    caldate = '2008-01-05';
    
    -- Find top 10 buyers by quantity.
    SELECT firstname, lastname, total_quantity 
    FROM   (SELECT buyerid, sum(qtysold) total_quantity
            FROM  sales
            GROUP BY buyerid
            ORDER BY total_quantity desc limit 10) Q, users
    WHERE Q.buyerid = userid
    ORDER BY Q.total_quantity desc;
    
    -- Find events in the 99.9 percentile in terms of all time gross sales.
    SELECT eventname, total_price 
    FROM  (SELECT eventid, total_price, ntile(1000) over(order by total_price desc) as percentile 
           FROM (SELECT eventid, sum(pricepaid) total_price
                 FROM   sales
                 GROUP BY eventid)) Q, event E
           WHERE Q.eventid = E.eventid
           AND percentile = 1
    ORDER BY total_price desc;
    ```
    
2.  (Optional) Open the Amazon Redshift console to review the queries that you ran. The **Queries** tab shows a list of queries that you ran over a time period you specify. By default, the console displays queries that have executed in the last 24 hours, including currently executing queries.
    
    *  Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console.aws.amazon.com/redshift/](https://console.aws.amazon.com/redshift/).
       
    *  In the cluster list in the right pane, choose `redshift-cluster-<NetID>`.
       
    *  Choose the **Queries** tab.
       
        The console displays list of queries you ran, as shown in the example following.  
        
        ![1567138663-fff5a9fa5dc1904e959bc21c2e4e359b](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138663-fff5a9fa5dc1904e959bc21c2e4e359b.png)
        
    *  To view more information about a query, choose the query ID link in the **Query** column or choose the magnifying glass icon.
       
        The following example shows the details of a query you ran in a previous step.  
        
        ![1567138663-039a6410688dc9666a1ce41fdcf13993](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138663-039a6410688dc9666a1ce41fdcf13993.png)



# [Step 7: Find Additional Resources and Reset Your Environment](#step-7-find-additional-resources-and-reset-your-environment)

When you have completed this tutorial, you can go to other Amazon Redshift resources to learn more about the concepts introduced in this guide. You can also reset your environment to the previous state. You might want to keep the sample cluster running if you intend to try tasks in other Amazon Redshift guides. However, remember that _you continue to be charged for your cluster as long as it is running_. To stop incurring charges, revoke access to the cluster and delete it when you no longer need it.

## [Where Do I Go From Here?](#where-do-i-go-from-here)

### [Additional Resources](#additional-resources)

We recommend that you continue to learn more about the concepts introduced in this guide with the following resources:

*   [Amazon Redshift Management Overview](https://docs.aws.amazon.com/redshift/latest/mgmt/overview.html): This topic provides an overview of Amazon Redshift.
*   [Amazon Redshift Cluster Management Guide](https://docs.aws.amazon.com/redshift/latest/mgmt/): This guide builds upon this _Amazon Redshift Getting Started_ and provides in-depth information about the concepts and tasks for creating, managing, and monitoring clusters.
*   [Amazon Redshift Database Developer Guide](https://docs.aws.amazon.com/redshift/latest/dg/): This guide builds upon this _Amazon Redshift Getting Started_ by providing in-depth information for database developers about designing, building, querying, and maintaining the databases that make up your data warehouse.

### [Resetting Your Environment](#resetting-your-environment)

When you have completed this tutorial, you should reset your environment to the previous state by doing the following:

*   Revoke access to the port and CIDR/IP address for which you authorized access:
    
    If you used the EC2-VPC platform to launch your cluster, perform the steps in [To Revoke Access from the VPC Security Group](#rs-gsg-how-to-revoke-access-vpc-security-group).
    
*   Delete your sample cluster. _You continue to incur charges for the Amazon Redshift service until you delete the cluster_. Perform the steps in [To Delete the Sample Cluster](#rs-gsg-how-to-delete-sample-cluster).
    

#### [To Revoke Access from the VPC Security Group](#to-revoke-access-from-the-vpc-security-group)

1.  In the Amazon Redshift console, in the navigation pane, choose **Clusters**.
    
2.  Choose **redshift-cluster-NetID** to open it, and make sure that you are on the **Configuration** tab.
    
3.  Under **Cluster Properties**, choose the VPC security group.  
    
    ![1567138801-0aefba43b066ce93e4fb633d80d744bf](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138801-0aefba43b066ce93e4fb633d80d744bf.png)
    
4. With the default security group selected, choose the **Inbound** tab and then choose **Edit**. 

   ![1567138801-a01d2f724fdf63fcd49ef79aad1727e8](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138801-a01d2f724fdf63fcd49ef79aad1727e8.png)

5. Delete the custom TCP/IP ingress rule that you created for your port and CIDR/IP address 0.0.0.0/0. Do not remove any other rules, such as the **All traffic** rule that was created for the security group by default. Choose **Save**.  

   ![1567138801-90ea1f031e6c72d355a1888ae482cd50](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138801-90ea1f031e6c72d355a1888ae482cd50.png)

#### [To Delete the Sample Cluster](#to-delete-the-sample-cluster)

1.  In the Amazon Redshift console, in the navigation pane, choose **Clusters**.
    
2.  Choose examplecluster to open it, and make sure that you are on the **Configuration** tab.
    
3.  In the **Cluster** menu, choose **Delete**.  
    
    ![1567138801-855389ca63e7ee4073b5623aa52f3d40](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138801-855389ca63e7ee4073b5623aa52f3d40.png)
    
4. In the **Delete Cluster** window, for **Create snapshot**, choose **No** and then choose **Delete**.  

   ![2019-08-30_21-26](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_21-26.png)

5. On the cluster details window, the **Cluster Status** displays that the cluster is being deleted. 

   ![2019-08-30_21-27](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_21-27.png)
   
   ![1567138801-bf08a1a1a746936e4907e4b10f5019c3](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/1567138801-bf08a1a1a746936e4907e4b10f5019c3.png)



# [Screenshots for submission](#screenshots)

1. Query editor (Must include Cluster name `redshift-cluster-<NetID>`)

   ![2019-08-30_19-10](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-10.png)

2. SQL workbench (Must take the whole screen of SQL workbench)

   ![2019-08-30_19-46](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-46.png)

3. Details of the most recent query (Must include `redshift-cluster-<NetID>`)

   ![2019-08-30_19-48](https://github.com/liuhoward/teaching/raw/master/big_data/redshift/2019-08-30_19-48.png)

**note**: Put these screenshots in MS word and save it as pdf for submission.

# Reference

[1] [https://docs.aws.amazon.com/redshift/index.html](https://docs.aws.amazon.com/redshift/index.html)

[2] [https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)



