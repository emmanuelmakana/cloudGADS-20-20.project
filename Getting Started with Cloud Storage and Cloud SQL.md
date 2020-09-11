
# Getting Started with Cloud Storage and Cloud SQL

# Objectives
    1. Create a Cloud Storage bucket and place an image into it.
    2. Create a Cloud SQL instance and configure it.
    3. Connect to the Cloud SQL instance from a web server.
    4. Use the image in the Cloud Storage bucket on a web page.

# STEP 1 :  Deploy a web server VM instance

    1. Create a VM instance
        -gcloud compute instances create bloghost --image Debian  9 --image-family Debian 9 --image-project [PROJECTID]

    2. Enable Firewall rule to allow http traffic in the vm
        -gcloud compute firewall-rules create HTTP-RULE --allow http
    
    3. Click Management, security, disks, networking, sole tenancy to   open that section of the dialog and add the following startup script
         -apt-get update
         -apt-get install apache2 php php-mysql -y
         -service apache2 restart

    4. Click create VM and leave the other settings as defaults.
   
    5. On the VM instances page, copy the bloghost VM instance's internal and external IP addresses to a text editor for use later


# STEP 2 : Create a Cloud Storage bucket using the gsutil command line

    1. On the Google Cloud Platform menu, click Activate Cloud Shell

    2. For convenience, enter your chosen location into an environment variable called LOCATION. Enter one of these commands:
        --export LOCATION=US

    3. Create a bucket with a global unique name i.e. use the project ID with the following command
        --gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

    4. Retrieve a banner image from a publicly accessible Cloud Storage location:
        --gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
    
    5. Copy the banner image to your newly created Cloud Storage bucket
        --gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

    6. Modify the Access Control List(ACLs) of the object you just created so that it is readable by everyone:
        --gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

# STEP 3 : Create the Cloud SQL instance

    1. In the console navigate to SQL and create an SQL instance, choose a db engine MYSQL
        ==> gcloud sql instances create prod-instance --database-version=MYSQL_5_7 --tier=db-n1-standard-1 --region=us-central1 --root-password=password123

    2. Click on the name of the instance, blog-db, to open its details page and copy the ip address

    3. Add a user on the details page and save the password.
    4. Cick the connections tab and wait to add a network
        --name should be webfrontend
        --Network should be the public external ip address of the VM instance created at the begining
        
# STEP 4 : Configure an application in the Compute Engine instance to use Cloud SQL

    1. In the VM instance created aboved ssh into it via the ssh command on the panel
    2. In the ssh session change the working directory to the root document of the webserver
        --cd /var/www/html
    3. Use the nano editor to the index.php file
        --sudo nano index.php
    4. Paste this content 
            [<html>
            <head><title>Welcome to my excellent blog</title></head>
            <body>
            <h1>Welcome to my excellent blog</h1>
            <?php
            $dbserver = "CLOUDSQLIP";
            $dbuser = "blogdbuser";
            $dbpassword = "DBPASSWORD";
            // In a production blog, we would not store the MySQL
            // password in the document root. Instead, we would store it in a
            // configuration file elsewhere on the web server VM instance.

            $conn = new mysqli($dbserver, $dbuser, $dbpassword);

            if (mysqli_connect_error()) {
                    echo ("Database connection failed: " . mysqli_connect_error());
            } else {
                    echo ("Database connection succeeded.");
            }
            ?>
            </body></html>]
    5. Exit and save the progress before exit
            -- CTRL + O    to save
            -- CTRL + X    to exit the editor

    6. Restart the webserver
            --sudo service apache2 restart
    7. Test the database connection
            -- Open a new web browser tab and paste into the address bar your bloghost VM instance's external IP address followed by /index.php
            -- The output should be [Database connection failed: ...]
    8. SSH to the vm and return to the root of the document
            --In the text editor replace the CLOUDSQLIP with the Cloud SQL instance Public IP address
            --In the text editor replace the DBPASSWORD with the Cloud SQL database password we created.
            --Save and exit and restart the webserver
                --sudo service apache2 restart
    9. Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:
            --Database connection succeeded.
    
# STEP 5 : Configure the application in the Compute Engine instance to use a Cloud Storage object

    1. Navigate to the Storage section in the GCP console and select the storage bucket created with the project ID.
    2. Copy the link address of the obect in the storage bucket
    3. SSH into the VM 
    4. Navigate to the root document of the webserver
        --cd /var/www/html
    5. Use the nano editor to edit the index.php file and add to the <h1> and <img> tag with the link address copied from the storage bucket of the obect.

    6. The line should be 
        -- <img src='https://storage.googleapis.com/qwiklabs-gcp-0005e186fa559a09/my-excellent-blog.png'>
    7. Save and exit the editor
        -- CTRL + 0 TO SAVE
        -- CTRL + X TO EXIT

    8. Restart the webserver
        -- sudo service apache2 restart
    9. Reload the browser tab and the image will be included in the output