---
title: "WordPress on App Service Public Preview"
author_name: "Abhishek Reddy"
tags:
    - PHP
---

We are happy to announce the public preview of the [WordPress offering on App Service](https://ms.portal.azure.com/#create/WordPress.WordPress). This offering will enable you to deploy and host new WordPress websites with ease.

Check out the following docs to learn more about deploying a WordPress website on AppService: [Quickstart: Create a WordPress site - Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/quickstart-wordpress)

For more details refer to [our blog post on MS Techcommunity](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/the-new-and-better-wordpress-on-app-service/ba-p/3202594)

## Known limitations
- In the preview period, WP installation may take up to 10 minutes. We're working to improve the deployment time.

## WordPress Linux Hosting Plans
The latest update provides you with four hosting plans. Hosting plans and SKUs are detailed below:

|**Hosting plan** | **Web app** | **Database (MySQL Flexible Server)** |
|--------------|------------|--------|
|Basic | B1 | Burstable, B1s |
|Development | S1 | Burstable, B1ms |
|Standard | P1V2 | Burstable, B2s |
|Premium | P1V3 | Gen Purpose, D2ds_v4 |

## Plugins
The new experience ships with two pre-installed plugins, W3TC and WP Smush. 
W3TC plugin pre-installed. It uses Redis cache to process requests faster. WordPress pages, objects, and DB objects are cached to improve performance.
WP Smush will compress images while retaining optimal quality. This will improve load times and increase WordPress performance.

## Changing MySQL Database Password
WordPress deployment creates an AppService and a MySQL database server, under the same resource group. Login credentials for the MySQL server are generated randomly during deployment process.  
 
Database connection details are configured into WordPress via ‘Application Settings’ option available in the AppService. Note that you can retrieve the database connection details from Application Setting section in case you forgot to note them down during the creation time.  
 
First, go to the MySQL resource corresponding to your WordPress deployment, and click on ‘Reset Password’ option as shown below. Now enter the new password and click on Save. Wait until the action is completed. 
 
 
Then navigate to the Configuration section of your AppService and update the Application Settings corresponding to the database connection details. Once you update the values, click on Save and wait for app to get restarted. The settings are as follows 
 
- DATABASE_HOST 
- DATABASE_NAME 
- DATABASE_USERNAME 
- DATABASE_PASSWORD 
 
## Changing WordPress Admin Password 
 
WordPress is installed on the AppService with the admin credentials provided by the user during the create process. These details are also added to the Application Settings of the AppService for initial deployment purpose. You can retrieve the credentials from here in case you forgot to note them during the creation time. 
 
Note that changing the admin credentials values in Application Settings after the deployment doesn’t update the same in WordPress, meaning that it does not actually changes the credentials. These App Settings only meant for initial deployment purpose and, also to help the users note them down for the first time. 
 
Actual update of admin credentials must be done via WordPress Admin dashboard. And this process doesn’t update the values back in the Application Settings of the AppService, as those parameters are out of sync once the app is created.  
 
Go to the admin dashboard of your WordPress site by using the following link format https://<yourapp>.azurewebsites.net/wp-admin. It will prompt you to login. Use the credentials you have entered during the create process to login to the dashboard. 
 
Then navigate to Users section as shown below and move your cursor over the user entry you want to make changes to and click Edit option which appears there.  
    
Set the new password using the option provided there and click on “Update Profile” to save the changes.     

## Environment variables
    
|Application Setting | Scope | Value | Max | Description |
|-------------|-------------|-------------|---------------|--------------------|
|WEBSITES_CONTAINER_START_TIME_LIMIT |Web app | 900 | -  | The amount of time the platform will wait (for the site to come up) before it restarts your container. WP installation takes around 5-10 mins after the AppService is deployed. By default, timeout limit for Linux AppService is 240 seconds. So, overriding this value to 900 seconds for WordPress deployments to avoid container restarts during the setup process. This is a required setting, and it is recommended to not change this value.  |
|WEBSITES_ENABLE_APP_SERVICE_STORAGE|Web App|true|-|When set to TRUE, file contents are preserved during restarts. |
|WP_MEMORY_LIMIT|WordPress|128M|512M|Frontend or general wordpress PHP memory limit (per script). Can't be more than PHP_MEMORY_LIMIT|
|WP_MAX_MEMORY_LIMIT|WordPress|256M|512M|Admin dashboard PHP memory limit (per script). Generally Admin dashboard/ backend scripts takes lot of memory compared to frontend scripts. Can't be more than PHP_MEMORY_LIMIT.|
|PHP_MEMORY_LIMIT|PHP|512M|512M|Memory limits for general PHP script. It can only be decreased.|
|FILE_UPLOADS|PHP|On|-|Can be either On or Off. Note that values are case sensitive. Enables or disables file uploads. |
|UPLOAD_MAX_FILESIZE|PHP|50M|256M	Max file upload size limit. Can be increased up to 256M.|
|POST_MAX_SIZE|PHP|128M|256M|Can be increased up to 256M. Generally should be more than UPLOAD_MAX_FILESIZE.|
|MAX_EXECUTION_TIME|PHP|120|120|Can only be decreased. Please break down the scripts if it is taking more than 120 seconds. Added to avoid bad scripts from slowing the system.|
|MAX_INPUT_TIME|PHP|120|120|Max time limit for parsing the input requests. Can only be decreased.|
|MAX_INPUT_VARS|PHP|10000|10000|-|
|DATABASE_HOST|Database|-|-|Database host used to connect to WordPress.|
|DATABASE_NAME|Database|-|-|Database name used to connect to WordPress.|
|DATABASE_USERNAME|Database|-|-|Database username used to connect to WordPress.|
|DATABASE_PASSWORD|Database|-|-|Database password used to connect to WordPress.|

WordPress installation simple, just provide the desired admin email, username, and password and everything else is taken care of. The latest update also brings significant performance enhancements like integrated caching and image compression.
