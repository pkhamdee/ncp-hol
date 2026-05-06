# Nutanix Unified Storage

# [#](#objects-cli-and-scripting) Objects: CLI And Scripting

## [#](#accessing-objects-from-the-cli) Accessing Objects From The CLI

While tools like the Objects Browser help to visualize how data is accessed within an object store, Objects is primarily an object store service that is designed to be accessed and consumed using S3 (Simple Storage Service) APIs.

Amazon's S3 is the largest public cloud storage service, and subsequently their S3 API has become the de-facto standard object storage API due to developer and ISV adoption. Objects provides an S3 compliant interface to allow for maximum portability, as well as support for existing "cloud native" applications.

You will leverage `s3cmd` in this exercise to access your buckets using the CLI.

You will need the **Access Key** and **Secret Key** for the user created earlier in this lab.

### [#](#setting-up-s3cmd-cli) Setting Up s3cmd (CLI)

This section of the lab is done using Linux Tools VM.

1.  SSH into **`User##`\-LinuxTools** using the following credentials.
    
    -   **Username** - `root`
    -   **Password** - `nutanix/4u`
2.  Enter `s3cmd --configure` and fill in the following to configure access to the Object Store.
    
    Note
    
    Press **Enter** for anything not specified below to submit the default.
    
    -   **Access Key** - _Access Key_
        
    -   **Secret Key** - _Secret Key_
        
    -   **Default Region \[US\]** - us-east-1
        
    -   **S3 Endpoint \[s3.amazonaws.com\]** - `OBJECT-STORE-IP`
        
    -   **DNS-style bucket+hostname:port template for accessing a bucket \[%(bucket)s.s3.amazonaws.com\]** - `OBJECT-STORE-IP:80`
        
    -   **Use https protocol \[Yes\]** - `No`
        
    -   **Test access with supplied credentials?** - `Y`
        
    -   **Save settings? \[y/N\]** - `Y`
        
        ![](/unified-storage/assets/1.1964a4c4.png)
        
3.  Execute `nano .s3cfg` to edit the file. Scroll down and modify the **signature\_v2** entry from `False` to `True`.
    
4.  Save the file by pressing \[CTRL\]\[X\]. Type `Y` and press **Enter** to save the file, and press **Enter** again to save the file using the existing name.
    

### [#](#create-a-bucket-and-add-objects-to-it-using-s3cmd-cli) Create A Bucket And Add Objects To It Using s3cmd (CLI)

1.  Now let's use s3cmd to create a new bucket called **`user##`\-cli-bucket**.
    
2.  From the same Linux command line, execute the `s3cmd mb s3://user##-cli-bucket` (ex. s3cmd mb s3://user01-cli-bucket). The expected output is:
    

```
Bucket 's3://user##-cli-bucket/' created
```

3.  Execute the `s3cmd ls` command to list your buckets.
    
4.  Execute `s3cmd ls | grep user##` (ex. s3cmd ls | grep user01) to view only your buckets.
    
    Now that we have a new bucket let's upload some data.
    
5.  Execute the following commands. You can execute these at the same time or one by one. The text after the `#` explains what the command does and will not be executed.
    
    ```
    curl http://10.42.194.11/hol/unified-storage/SampleData_Small.zip -O -J -L #downloads the sample data
    mkdir sample-pictures #creates a new folder
    unzip -j SampleData_Small.zip *.png -d sample-pictures #extracts only the picture files
    ```
    
6.  Execute `ls sample-pictures` to list the images within the sample-pictures folder.
    
7.  Run the following commands to upload the **login.png** image to your bucket.
    
    ```
    cd sample-pictures #changes directory to sample-pictures
    s3cmd put --acl-public --guess-mime-type login.png s3://user##-cli-bucket/login.png #uploads the login.png file to your user##-cli-bucket
    ```
    
    The expected output is:
    
    ```
    2023-04-17 19:19     71697   s3://user##-cli-bucket/login.png
                           DIR   s3://user##-test-bucket/Pictures/
    2023-04-17 18:00       374   s3://user##-test-bucket/version.rtf
    ```
    
8.  Execute the `s3cmd ls` command to list all objects across all buckets or `s3cmd ls | grep user##` to only list objects within your buckets.
    

## [#](#creating-and-using-buckets-from-scripts) Creating And Using Buckets From Scripts

In this exercise, you will use **Boto 3**, the AWS SDK for Python, to manipulate your buckets using Python scripts.

### [#](#listing-and-creating-buckets-with-python) Listing And Creating Buckets With Python

In this exercise, you will modify a sample script to match your environment, listing all the buckets available to that user. You will then modify the script to create a new bucket using the existing S3 connection.

1.  Execute `nano list-buckets.py` and paste in the script below. Before saving the script, you must modify the Objects IP address, access\_key\_id, and secret\_access\_key\_id values.
    
    ```
    #!/usr/bin/python
    
    import boto3
    import warnings
    warnings.filterwarnings("ignore")
    
    endpoint_ip="OBJECT-STORE-IP" #Replace this value
    access_key_id="ACCESS-KEY" #Replace this value
    secret_access_key="SECRET-KEY" #Replace this value
    endpoint_url= "https://"+ endpoint_ip +":443"
    
    session = boto3.session.Session()
    s3client = session.client(service_name="s3", aws_access_key_id=access_key_id, aws_secret_access_key=secret_access_key, endpoint_url=endpoint_url, verify=False)
    
    # list the buckets
    response = s3client.list_buckets()
    
    for b in response['Buckets']:
      print (b['Name'])
    ```
    
2.  Save the file by pressing \[CTRL\]\[X\]. Type `Y` and press **Enter** to save the file, and press **Enter** again to save the file using the existing name.
    
3.  Execute `python list-buckets.py` to run the script. Verify that the output lists any buckets you have created using your first user account.
    

### [#](#uploading-multiple-files-to-buckets-with-python) Uploading Multiple Files To Buckets With Python

1.  Execute the following to create one hundred 1KB files to be used as sample data for uploading:
    
    ```
    cd ..;mkdir sample-files
    for i in {1..100}; do dd if=/dev/urandom of=sample-files/file$i bs=1024 count=1; done
    ```
    
    While the sample files contain random data, these could be log files that need to be rolled over and automatically archived, surveillance video, and employee records.
    
2.  Execute `nano list-buckets.py` and paste the below to create a new script.
    
    ```
    #!/usr/bin/python
    
    import boto3
    import glob
    import re
    import warnings
    warnings.filterwarnings("ignore")
    
    # user-defined variables
    endpoint_ip= "OBJECT-STORE-IP" #Replace this value
    access_key_id="ACCESS-KEY" #Replace this value
    secret_access_key="SECRET-KEY" #Replace this value
    bucket="BUCKET-NAME-TO-UPLOAD-TO" #Replace this value
    name_of_dir="sample-files"
    
    # system variables
    endpoint_url= "https://"+endpoint_ip+":443"
    filepath = glob.glob("%s/*" % name_of_dir)
    
    # connect to object store
    session = boto3.session.Session()
    s3client = session.client(service_name="s3", aws_access_key_id=access_key_id, aws_secret_access_key=secret_access_key, endpoint_url=endpoint_url, verify=False)
    
    # go through all the files in the directory and upload
    for current in filepath:
        full_file_path=current
        m=re.search('sample-files/(.*)', current)
        if m:
          object_name=m.group(1)
        print("Path to File:",full_file_path)
        print("Object name:",object_name)
        response = s3client.put_object(Bucket=bucket, Body=full_file_path, Key=object_name)
    ```
    
3.  Save the file by pressing \[CTRL\]\[X\]. Type `Y` and press **Enter** to save the file, and press **Enter** again to save the file using the existing name.
    
    The [put\_objectopen in new window](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html?highlight=put_object#S3.Bucket.put_object) method is used for the file upload. Optionally this method can be used to define the metadata, content type, permissions, expiration, and other critical information associated with the object.
    
    Core S3 APIs resemble RESTful APIs for other web services, with PUT calls allowing for adding objects and associated settings/metadata, GET calls for reading objects or information about objects, and DELETE calls for removing objects.
    
4.  Execute `python upload-files.py` to run the script.
    
5.  Execute `s3cmd ls s3://user##-cli-bucket/` to verify that the sample files are available. A portion of the output is shown in the example below.
    
    ```
    ('Path to File:', 'sample-files/file88')
    ('Object name:', 'file88')
    ('Path to File:', 'sample-files/file89')
    ('Object name:', 'file89')
    ('Path to File:', 'sample-files/file90')
    ('Object name:', 'file90')
    ('Path to File:', 'sample-files/file91')
    ('Object name:', 'file91')
    ```
    

Similar S3 SDKs are available for languages including Java, JavaScript, Ruby, Go, C++, and others, making it very simple to leverage Nutanix Buckets using your language of choice.

# [#](#takeaways) Takeaways

-   Nutanix Objects provides a simple and scalable S3-compatible object storage solution optimized for backup and archive use cases and cloud-native and big data analytics workloads (data lake).
-   Nutanix Objects can be deployed on AHV or ESXi.
-   Nutanix Objects is deployed and managed from Prism Central.