# Migrating a Database to Amazon RDS: Step-by-Step Guide  

## Introduction  
As the business grows, so does the need for scalable and efficient technology infrastructure. Currently, the café hosts its web server, database, and application code on an Amazon EC2 instance. However, maintaining a database on EC2 involves challenges like manual backups, patching, and database management, which can be time-consuming. To streamline operations, the café has decided to migrate its database to Amazon Relational Database Service (RDS). RDS provides a fully managed solution that automates backups, scaling, and patching, making it easier to focus on application development.
  
## Step 1: Creating an RDS Instance
To begin, we'll set up a new RDS instance using the MariaDB engine. We'll choose the Dev/Test template for cost-effectiveness, with configurations like a db.t3.micro instance class and 20 GiB of general-purpose SSD storage. For security, we'll ensure the database is not publicly accessible by using the lab-db-subnet-group. Additionally, we'll configure the dbSG security group to control access. Once the configurations are set, initiate the creation of the RDS instance; it may take a few minutes to complete, so proceed to the next steps in the meantime.

![image](https://github.com/user-attachments/assets/41e6d38a-3bc3-4681-845a-2f9c60eb0380)


## Step 2: Analyzing the Existing Café Application Deployment
The current café application runs on an EC2 instance named CafeServer. Before making changes, let's access the application to verify its current functionality. Open a browser and visit http://<public-ip-address>/cafe to check the Menu and Order History pages. To securely connect to the EC2 instance, use AWS Systems Manager Session Manager, which eliminates the need for SSH keys. After starting a session, ensure you're logged in as ec2-user to access the necessary files and configurations.
![image](https://github.com/user-attachments/assets/d16039cf-c6a7-47dd-9615-268c9df2250c)


## Step 3: Exporting Data from the Old Database
To migrate the database, we first need to confirm the existing setup on the EC2 instance. Use the command line to check the status of the MariaDB service and connect to the database using the root credentials. You can retrieve the database password from the Parameter Store entry named /cafe/dbPassword. Once connected, use SQL commands to explore the existing tables and data. To prepare for the migration, use mysqldump to export the entire database into a file called CafeDbDump.sql. This file will serve as our backup for importing into the new RDS instance.

![image](https://github.com/user-attachments/assets/92801e32-3597-4cda-8af8-10b5962ff371)
![image](https://github.com/user-attachments/assets/09ee0b4f-3fb9-4142-a369-d738ce8a7b3b)



## Step 4: Working with the New RDS Database
After your RDS instance is up and running, we need to configure access from the EC2 instance. Update the dbSG security group to allow inbound traffic on port 3306 from the EC2 instance's security group. Use the nmap tool to confirm that the port is accessible. Once that's done, connect to the new RDS database using the MySQL client and verify that you can successfully access it with the new credentials. This connection will enable us to transfer data into the new database.

![image](https://github.com/user-attachments/assets/3cd50492-eeb2-4a1e-a2f5-60bc102a55fb)


## Step 5: Importing Data to the RDS Instance
With a secure connection to the RDS instance established, we can now import the data exported from the EC2 database. Use the MySQL client to load the CafeDbDump.sql file into the RDS database. This process will recreate all tables and populate them with existing data. After importing, verify the success of the operation by checking that all tables and records are present in the new RDS instance using simple SQL queries.

![image](https://github.com/user-attachments/assets/f960195b-f9fb-453f-a6ee-40022b89f5f3)


## Step 6: Updating the Café Application to Use the RDS Database
The café application fetches its database connection details from AWS Systems Manager Parameter Store. To switch the application to the new RDS database, update the parameters for dbUrl, dbUser, and dbPassword to reflect the RDS instance’s endpoint and credentials. No changes are needed for other parameters like dbName or timeZone. After updating these configurations, restart the web application and test its functionality to ensure it connects to the new database. Confirm that all existing and new orders are correctly reflected in the Order History section.

![image](https://github.com/user-attachments/assets/45a1bd81-f777-4706-90dc-d5dc67af96fe)


## Step 7: Shutting Down the Old Database
With the migration completed and the application successfully using RDS, it’s time to decommission the old database on the EC2 instance. To avoid conflicts and unnecessary resource usage, stop the MariaDB service on the EC2 instance. This ensures that all database operations are now handled exclusively by Amazon RDS, reducing maintenance overhead and improving database reliability.

![image](https://github.com/user-attachments/assets/329a0d27-7d83-4c95-a352-39608b1bb137)


## Skills Demonstrated  
- Amazon RDS Configuration and Management  
- Secure Database Migration and mysqldump Usage  
- Application Integration with RDS  
- AWS Systems Manager and Security Group Configuration  

## Conclusion  
By migrating the café's database to Amazon RDS, we've simplified database management, enhanced scalability, and reduced administrative overhead. The fully managed RDS environment offers automated backups, easy scaling, and regular updates, allowing the café team to focus more on business operations rather than infrastructure maintenance.  

## Author  
**Gomolemo Hlatshwayo**  
- [LinkedIn](https://www.linkedin.com/in/gomolemo-hlatshwayo-2b844522a/)  
- [GitHub](https://github.com/gomolemo-sudo)  
