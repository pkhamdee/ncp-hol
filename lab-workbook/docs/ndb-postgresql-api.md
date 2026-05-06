# Nutanix Database Service (NDB) Lab

# [#](#ndb-rest-api-explorer) NDB REST API Explorer

## [#](#overview) Overview

In this lab, you will explore the NDB REST API Explorer.

## [#](#using-the-ndb-rest-api-explorer) Using the NDB REST API Explorer

NDB provides a fully documented _API-first_ architecture to allow integration with automation and orchestration of its functions through external tools. Similar to Prism, NDB also provides a Rest API Explorer to discover and test API functions quickly.

1.  From the menu bar, select **Admin > REST API Explorer** from the top right.
    
    ![](/ndb/assets/29.210e9762.png)
    
2.  Expand the different categories to view the available operations, including registering Nutanix clusters, registering and provisioning databases, cloning, refreshing databases, updating profiles and SLAs, and getting the operation and alert information.
    
3.  As a simple test, expand **Databases > GET /databases**.
    
    This function returns JSON containing details regarding all registered and provisioned databases and requires no additional parameters.
    
4.  Click **Try it out > Execute**.
    
    ![](/ndb/assets/30.ae0edb4c.png)
    
    You should receive a JSON response body similar to the image below.
    
    ![](/ndb/assets/32.888a26de.png)
    
    This API can create powerful workflows using tools like Nutanix Calm, ServiceNow, Ansible, or others. For example, you could provision a Calm blueprint containing the web tier of an application and use a Calm eScript to invoke NDB to clone an existing database and return the IP of the newly provisioned database to Calm.