# MIS 587 Guide to Data Warehouse SSIS Installation



Please follow below steps to setup and install below software, make sure your hard drive has at least 20GB free space: 

(For MAC users, ask IT to install Windows 10 and perform below steps or borrow a windows laptop from UA library)


[TOC]

### 1. Visual Studio Community
Download Visual Studio Community  from the Developers Tool section of Microsoft Imagine. Install the visual studio without Add-On’s (around 600 MB size) 

https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-relnotes

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image001.png)

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image003.png)

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image005.png)




### 2. SQL Server Express
Download SQL Server Express from https://www.microsoft.com/en-us/download/details.aspx?id=55994

Click on `Customize`. Follow the instructions to set up SQL Server. Make a note of Server Instance Name. Choose authentication as Windows Authentication 

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image007.png)

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image009.png)

 

Click “New SQL Server stand-alone installation or add features …”

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image011.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image013.png)



If have Windows Firewall warning, You can turn off firewall in control panel or enable the required ports following the link  

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image015.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image017.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image019.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image021.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image023.png)



![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image025.png)

 

**Attention:** if you encounter errors, you may check the service `windows modules installer` is running following the **[7. Note](#7. Note)** part in the end of this tutorial.

### 3.  SQL Server Management studio
Download SQL Server Management studio (latest version) from https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms

Click on `Download SQL Server Management Studio (SSMS)`

Execute the installation files 


![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image027.png)

 

### 4. SQL Server Data Tools
Download SQL Server Data Tools (Download SSDT for Visual Studio 2017) from https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt
find `SSDT standalone installer`
https://docs.microsoft.com/en-us/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image029.png)

 

### 5. After SQL Server Data Tools has been installed, open Visual Studio 2017. 

After opening Visual Studio 2017, choose `File -> New -> Project`, from left sidebar, you should be able to find Installed -> Business Intelligence -> Integration Services, and create new project.

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image031.png)

 

### 6. Open Microsoft SQL server Management Studio. 
Choose server type as Database Engine, Appropriate server name and authentication as Windows Authentication. Click Connect. You should get an object explorer window. On drilling down, you can see various folders, one of which is databases.

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image033.png)

 

![img](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/clip_image035.png)

 

 

### 7. NOTE

1. For MAC users, ask IT to install Windows or borrow a Windows laptop from UA library and perform above steps 
2. If you use Visual Studio 2019, please refer this [link](https://blog.pragmaticworks.com/visual-studio-2019-bi-design-tool-extensions) to implement installation and tutorial.

3. If you are not able to find the instance name/ Server name to connect to database engine. Follow below steps: 

Navigate to: 

Start->All Program->Microsoft SQL Server-> Configuration Tools-> SQL Server Configuration Manager->SQL Server services 

Verify the name of server instance. Check the state of Server instance. It should be in running state

4. If you encounter error when you install SQL Server Express, please check the service `Windows module installer` is running as following:

* press `win + r`, input 'services.msc'

 ![1](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/1.PNG)

* find the service `Windows module installer` and double click

  ![2](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/2.PNG)

* make sure the `service status` is `running` or else click `start`.

  ![3](https://github.com/liuhoward/teaching/raw/master/business_intelligence/install_assets/3.PNG)

5. You can use the following or any other such video as a guide to install

https://www.youtube.com/watch?v=UcsItGq3mmM
