# MIS 587 Guide to Data Warehouse Creation using SSIS Package



[TOC]

### Overview:

In this exercise, we are going to build a data warehouse for the given data sample. We will be using SQL Server Integration Services tool to build a data warehouse. By the end of this exercise you should be able to design a data warehouse schema, implement the design using SSIS tool and perform analysis/ reporting using SSAS/ SSRS.

### Problem Statement: 

A physical warehouse near Tucson has inventory belonging to various categories which is shipped to many countries around the world by three shippers. A data warehouse needs to be built to maintain a single source of truth relating to the information about the goods shipped. They want to use this data to make business decisions.

### Solution:

* Analyze the data sample

* Design appropriate schema for data warehouse

* Build Data warehouse

* Design dashboards and reports to help Inventory/ Superstore make their business decisions (Out of Scope of this Lab session)

### Pre-requisite: 

Install SQL Server Express, SQL Server Management studio, SQL Server Data Tools, Visual Studio community   following the [Installation tutorial](https://github.com/liuhoward/teaching/blob/master/business_intelligence/Datawarehouse_Installation.md)





### Description step by step:

We will extract tables from [Northwind database](https://github.com/liuhoward/teaching/blob/master/business_intelligence/Northwind-Sample-Database-Diagram.pdf), import data into Northwind_DW database and build star schema.

1. Open Microsoft SQL server Management Studio.

   ![1](MIS587_data_warehouse_example1.assets/1.PNG)

2. Build Northwind database as data source:

click `New Query`, copy sql code from the [link](https://raw.githubusercontent.com/liuhoward/teaching/master/business_intelligence/instnwnd.sql.txt), paste the code into query editor, click `Execute`, you will find a database `Northwind` in left sidebar. This contains Inventory source tables with data. Relevant attributes for each entity are present in the tables.

![2](MIS587_data_warehouse_example1.assets/2.PNG)                                

3. Build a database named ‘Northwind_DW’ with empty tables for the Northwind Data warehouse using [sql code](https://raw.githubusercontent.com/liuhoward/teaching/master/business_intelligence/Northwind_DW_init.sql.txt) like previous step. Refresh database list, you will find a new database `Northwind_DW`.

![3](MIS587_data_warehouse_example1.assets/3.PNG)



Now, Microsoft Visual Studio will be used to load the data to the northwind data warehouse from the Northwind database, after doing certain transformations.

4. Open Microsoft Visual Studio. Choose `File-> New->Project`

   ![1580888098115](MIS587_data_warehouse_example1.assets/1580888098115.png)

5.  A window `New Project` pops up. Choose `Business Intelligence->Integration Services Project`

![4](MIS587_data_warehouse_example1.assets/4.PNG)

6. You should get a blank screen as below:

![5](MIS587_data_warehouse_example1.assets/5.PNG)



Using Microsoft Visual Studio, Microsoft SQL server is accessed for the data integration of the Northwind Data warehouse.

7. From left sidebar, Drag and drop `Data Flow task` onto the workspace. Rename it as `Order Dimension`

![7](MIS587_data_warehouse_example1.assets/7.PNG)

8. Double Click on the `Order Dimension` data flow. You should see a page like below open.

![8](MIS587_data_warehouse_example1.assets/8.PNG)

9.  On the left-hand bar, Drag `OLE DB Source` from `Other Sources`  on to the workspace and `OLE DB Destinations` under `Other Destinations` onto the workspace. Join both using the blue arrow.

   ![9](MIS587_data_warehouse_example1.assets/9.PNG)

12) Double Click on the “OLE DB Source”. 

   

13) Click “New” Button to create a new connection manager. This creates a new connection to a database installed on Microsoft SQL server.

 

   

14) In the field “Server Name”, Type in the name of the Microsoft SQL Server installed on your computer. The name to choose is shown in the screenshot of Microsoft SQL server below.

   

 

15)  Next, under Select or enter a database name, choose “Northwind” Database. Click OK.

 

   

 

16)  Now choose “Orders” table under “Name of the table or view”. Click OK.

   

 

17) In the Data Flow workspace, double click on the “OLE DB destination” and Create a connection manager to the Northwind_DW Database. Repeat Steps 13-16 to create a new connection manager similar to what was done for the northwind database.

18) After the new connection manager has been set up, Choose “Dim Orders”. This is a dimension created to hold basic qualitative attributes about orders. Choose OK.

   

 

19) Repeat this procedure for supplier, category, shipper, product, employee dimensions. Then join them as shown below:

   

 

20)  The Inventory Fact table is loaded from the Inventory “order details” table. We already create an empty Fact table in Northwind_DW, foreign key placeholders are created, which will be populated later.

 

Similarly, drag ‘Data Flow Task’, rename it as ‘fact table’, double click, Like before, drag the DB source from the left pane, select ‘order details’ from Northwind database.

   

 

   

 

21) Next choose the Lookup task from the left pane and drag it into the workspace. Join using a blue arrow.

   

22) Click on the lookup icon. Choose the Northwind_DW connection manager. Choose “Use results of a SQL query”. And paste the query from the “Time” section in the below text file.

   

 

   

 

23) In the “columns” section of the editor, link orderID. Choose “Date Skey”. Click Ok. 

 

 

   

24) Rename the lookup as “Time Lookup”. Join with “OLE DB source” using blue line.

25) Choose another lookup and drag it onto the workspace, rename it as ‘order lookup’. Connect time lookup blue to the order lookup, choose ‘lookup match output’

   

Double click on it to open the editor. Choose Northwind_dw connection manager. Choose Dim orders dimension.     

In “columns” section of editor, link OrderID, choose ‘Order Skey’.

   

 

26) Choose another lookup and drag it onto the workspace, rename it as ‘product lookup’. Double click on it to open the editor. Choose Northwind_dw connection manager. Choose Dim Products dimension.

   

 

​    

 

27)  Similar to “Time” lookup, sql codes corresponding to each lookup are given in the “Dimension join codes.txt” file. create lookups for employee, category, supplier, shipper.

   

28)  Drag ‘OLE DB destination’, double click, choose ‘Fact Order Details’ from Northwind_DW:   

 

The Foreign keys in the inventory fact table are populated from the dimension tables by matching the primary keys from their respective dimension tables.

   

 

   

 

29)   Truncate the data warehouse database tables:

Click the Control Flow tab.

Drag ‘Execute SQL Task’ task onto the design surface of the Data Flow tab.

   

Right-click the task component and when click Edit.

In General tab, Configure your SQL Connection

   

 

Add the attached SQL query to truncate all tables before running the package next to ‘SQLStatement.

   

   

 

   

 

 

30) Go to control flow and click ‘start’. You should get a page like below:

   

 

(you can stop it by clicking red square button)

   

31) Go to Microsoft SQL server. The tables in the Northwind_DW database must have values now.

   







































































===============================================================================================





