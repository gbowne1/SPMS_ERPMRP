    General information (Application Version, Name(s), Contacts, etc.)
    Database documentation (Setup Document from Vendor, Database Diagram, Data Classification, etc.)
    Database setup requirements (Hardware, Software, Networking, etc.)
    Database growth and future disk space requirements 
      
    Backup and Recovery requirements
    Security settings
    Remote access requirements
    Connections strings and other database related application settings
    Application operational information (For example: Automated processes run at night)
    

Questions for the Database Requester

Every software application is different and some of the questions may not be applicable to every single application. So, when DBAs send the questions to a database requestor we usually ask them to make the best effort in filling out the questionnaire. Then we review the form and if any important information is missing we ask them to complete specific sections of the request form.
General Information

    What is the name of the requested application and version?
      We have not decided on the name of the application, other than SPMS ERP/MRP.  This app/database could potentially be of use to many different similar companies.
      
    
    All we need to do is ask the user what application version the database belongs to.
    What is the latest application version available?
    
    Provide vendor’s database diagram or other documentation that describes database objects.
        The database will contain typical ERP, MRP, manufacturing, CRM, HRIS/HRM, CMMS, Quality, etc. information
        Not a financial application, but contains customer information and some dedicated financial information (requires auditing of any access to the Customer table)
    
    Pre-requisites
   
     Microsoft SQL Server 2008-2017 Express 
     MySQL (??)
     PostgreSQL (??)
     MariaDB (??)
  
    Hardware requirements (SQL Server CPU, Memory, Storage, etc.)
        Runs on a single Dell PowerEdge R710 SFF Server w/4.8TB storage (8x 600gb), 288gb Max RAM and is also connected to a single Dell PowerVault MD1220 storage array
        chassis.
        
    How many concurrent users: We average 15 to 30 users logged in.
    What users/logins have to be created before application installation?
    For example:
        Application service account - Domain user (application service startup account or application pool account for the web application)
        Application service account - SQL Server Login
        Application support user or vendor's login needs db_owner access to the database for the application installation (remove after the installation)
   
   
   Contacts
    Project Manager (if project initiated) and/or Technical Lead contact.
       Myself
    Implementer(s) contact information (who will install an application/database) for each environment.
       Myself
    Application support contact(s) for each environment. Specify if different for server support (usually technical person) and for the application support (business person).
    Business owner of the application including department name (required for the Change Management approvals). Department name might be required for some of the databases statistics (for example, databases space used by department)
       Myself
    
    Provide multiple approvers if there are integrations involved.
    
    
    When is the database required?
    No specific end date requested.
    
    Test environment due date
    No specific end date requested.  However we would like to test the enviroment by 12:00:00 midnight GMT -8 12-31-2020.
    
    Production Date 
      Although no specific date was set, we would like to be live by 12:00:00 midnight GMT -8 12-31-2021.
    
    Capacity
      Indeterminate.  Although storage limit would be 40TB and still allow backup.
      
   What is the initial database size?
    I would like to plan for a minimum of 1TB and a maximum of 4-5TB.  Although size is not stringent but enough to allow for 1 year of growth.
    
    Provide database growth rate:
        What amount of data in MB\GB\TB or in Rows changes daily/weekly/monthly (provide one)? 
        How much data usually added each year (in MB\GB\TB)? Can you provide examples based on other customers statistics.
        Up till now it's been 1 to 4TB annually.
    
    Security
    What Authentication will be used by the application to connect to SQL Server?
   
        SQL Authentication
        Windows Authentication – Domain Login or Group (preferred)
        Some users could be managed through the application; only application service account required.
        
    Does the application require DBAs to create logins going forward? 
       No. I will be the application administrator to create logins used in the application, but a DBA or a SA can. At least till we can permanently assign a DBA and ERP/MRP Admin.
       
       What is the process of the user creation/deletion?
         User Password, User Name and Walidation
         
    Does the application require a service account and what database/SQL Server permissions will it need? 
    Yes, SYSADMIN server role is required for the day-to-day application functionality. Use minimal permissions principal.
    
    Are there any specific permissions that are required only for the application installation.
    These permissions are safe to remove/downgrade after the installation.
    
    Installation and Configuration
    Please provide vendor’s documentation (or URL to the document) with database setup steps.  Please create some basic documentation that is easily edited, readable.
    
    How should the database be initially created?
    
    The tables have already been created in Access and/or Excel.  However, It may need an empty database, application will create database objects and or retreive 
        There is vendor’s script to create database objects
        There are data files or a backup provided by the vendor to initialize (attach or restore) the database.
    
    What is the suggested database name? SPMS_ERPMRP_DB
    
    Does application support a custom database name? Yes, it should.
    
    List remote access requirements.
    For example:
        Do users from remote offices require direct access to the database (for example with SQL Server Management Studio)?  Yes.  Possibly a secure VPN or weblogin / firewalled user.
        Are there any firewall rules that have to be implemented to support these connections?  Yes.
        Are there any requirements for the offline data access (consider replication)?
        
    Are there any additional security considerations?
    
        Yes: very sensitive customer data, regulations require to keep database on a dedicated SQL Server with limited access
        Yes: External clients need access to the set of the tables through the external web site (consider replication or data export)
        Yes: Real time read only access required for the application support or developers (consider readable replicas with read-only intent)
   
   Backup and Recovery
    How often does the database change during the day/week/month?  Daily.  Almost every hour/minute/second.-
    We have:
        read-only
        configuration database (rarely changed)
        
    How much data loss is acceptable? 
    Case in point.  No acceptable data loss, but kept to minimum. 3 to 5 hours. We can manually enter missing data. Backups are done every 4 hours and a master backup every day at midnight.
    
    Are there any other systems that help to bring the database up to date after full backup is recovered? Yes,
 
        There are SQL scripts generated for an application every hour that could be executed to bring the database up to date
        Users may re-import missing data from/using Excel, Access, etc.
        Users will run one or two simple batch processes using an application.  
        
    Are there any requirements to keep databases in sync with integrated system(s)? Please provide details and steps to synchronize the data if both systems can not be recovered to the same point of time.
    For example:
        An application uses three databases and all of them have to be in-sync
        When Application "A" database is restored Application "B" database have to be restored from the same day/time backup
        Users create SSRS reports using an application. Both - SSRS reports have to be in-sync with the database as reports information is saved in the application's database.
        Application "C" depends on files located on the network share; have to be in-sync
        A database "D" has database link to an external database; data in both has to be in-sync. Data will be re-imported base on a timestamp from the external database in case of database "D" restore.
    
    Operational Information
    Does the application have any resource intensive tasks/operations that will run during the day or afterhours?  Somewhat yes. 
    For example:  Application runs items in a roll-up job at 11:00 PM which conflicts with one of the database maintenance tasks. The schedule needs to be coordinated with the DBAs to prevent database locks and long running jobs.
    
    How is the database configuration performed and where is connection information specified?
    This will help during future databases migrations to different SQL Servers.
    For example:
        There is an application configuration tool
        Database connection information is specified in web.config file (provide server name and file's path)
        ODBC connection on application server or desktop client
        Database setting are in *.ini file (provide the location of the file).
    
    Dependencies
        To be determined.
        
    Provide application server(s) names
        Not named yet.  Normal convention follows.
    
    Provide any dependencies on SQL Server features or services.
  
        SQL Server Reporting Services
        Partitioning
        Full-Text Search
        
    Non-SQL Server dependencies:
    
        There is a network folder that used by the application's bulk-insert operations and service account requires access to this folder
        Scripts that executed by SQL Server Agent Job/SSIS package located on a network drive
        TNSNAMES.ora file is on the network drive (required for data imports from Oracle to SQL Server)
        Integrations require another system (for example, Oracle database) to be available.

Note: The following information may not be available during initial requirements gathering and will be filled out by a DBA when database/application goes to Production.

    Database Server name/instance
    SPMS_ERPMRP_SQL
 
 Database Administrators' accesses.
    
        Using Windows Authentication (preferred) and SQL Server Management Studio
        By using VPN connection to the external server
        Using SQL Server Authentication (only when Domain Authentication is not available).
        
    Additional database user setup requirements:
        Read only (back end) database access for the application support person in the Test environment
        
    Audit setup (if applicable).
    For example:
        DDL trigger created to capture all views changes for the Delta_Prod database and send e-mails to the DBA ; F5 only happens occasionally and DBA has SA
        Extended events setup to capture all security changes
        SQL Server Audit setup required to track any databases changes


Last Updated: 2020-06-11
