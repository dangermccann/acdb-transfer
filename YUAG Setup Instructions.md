
# ACDB Transfer to YUAG

## ACDB Overview 
ACDB has three major components:
1. Client application written in Angular / Typescript
2. Node.js server application that exposes APIs used by the client
3. MySQL Database that stores all of the data.  

Note: images are stored in the filesystem in a subdirectory of the server application.  

To simplify the transfer of ACDB to YUAG, all three components have been packaged into a single AMI that will be shared with Yale.  


## ACDB Transfer Instructions 

The ACDB client, server and MySQL database, along with all data and images, have been packaged into an AMI (Amazon Machine Image) that is ready to be transferred into a Yale-owned AWS account.  AWS provides a simple way for AMIs to be shared between accounts, and we will use this sharing feature to execute the transfer to Yale.  We will need the account number of the AWS account we will be transfering ACDB to, and when we receive it, the AMI will be shared with that account.  

Please provide me with the AWS account number, and I will go ahead and share the AMI with that account.  

### Locate the AMI 
Once the AMI is shared, you will need to locate it in AWS.  
1. In the AWS Console, navigate to the EC2 service using the Services menu on the top of the page.
2. From the navigation menu on the left, select Images -> AMIs. 
3. Open the drop down menu labled "Owned by me" and select "Private images". 
4. Locate AMI with name "acdb-export" and select the checkbox for that AMI.  

### Create the Instance 
Next we will launch a EC2 instance from this AMI.  When the instance starts up, it will be running both the ACDB application and the MySQL database.  

1. With the "acdb-export" AMI selected, click on the Launch instance from AMI button.  
2. A page will appear that allows you to customize the instnace settings.  First, type in a meaningful name for the instance, such as "ACDB Server" or similar.  
3. Most of the remaining settings can be left as is.  The Instance type it will default to is t2.micro.  You can choose a larger instance type, such as t2.small, for better performance at a higher cost.  
4. Select the key pair you want to use from the drop down.  I will not have access to your key pair, and as such I will not be able to access or manage the instance once it has been launched.  
5. Under Network Settings make sure all three checkboxes are checked: 
    * Allow SSH traffic from [Anywhere]
    * Allow HTTPs traffic from the internet
    * Allow HTTP traffic fom the internet
6. Press the Launch Instance button


### Verify that ACDB has started up 

1. Wait for instance to start up.  You can manage the status from the Instances list.  
2. When the instance is ready, locate the public IP address of the instance by selecting it in the list.  
3. At this point if you navigate your web browser to the IP of the instance, you will see the login page for ACDB, but login attempts will fail.  
4. SSH to the instance to start up the ACDB server.  Open your SSH terminal application of choice, and using your private key, start an SSH session with the instance as follows: `ssh -i ./path/to/key.pem ubuntu@[ip address]`

5. Start the ACDB server as follows:  

```
cd acdb/acdb-server
nohup npm start &
```

6. With the server running you should be able to use DCDB.  Open browser to: `http://[ip address]/`

Log in and verify that all Lusoria, Items, images and other data is presented.  

### DNS Transfer 
If you would like to associate a domain name for ACDB, we have two options:
1. Transfer the existing domain (ac-db.com) to YUAG for you manage.  
2. Create a new DNS record and associate the A Record with the public IP address of the EC2 instance of the ACDB server.  

Please let me know how you would like to proceed.  

### SSL 
Once an appropriate DNS record is in place, the site should be secured with an SSL certificate.  I'd be happy to provide setup instructions for this if necessary.  

### Source code 
The source code for the ACDB application is in a private repository in Github.  I'd be happy to transfer ownership of the code to another Github account if you provide me the account details.  I will also provide the source code as a zip file along with these instructions.  

