# Implement Private Google Access and Cloud NAT

# OBJECTIVES
    1. Configure a VM instance that doesn't have an external IP address
    2. Connect to a VM instance using an Identity-Aware Proxy (IAP) tunnel
    3. Enable Private Google Access on a subnet
    4. Configure a Cloud NAT gateway
    5. Verify access to public IP addresses of Google APIs and services and other connections to the internet


# STEP 1 : CREATE A VM INSTANCE WITH NO EXTERNAL IP
    1. Create a VPC network and some firewall rules to allow tcp.(Required ports)
        ==> gcloud compute networks create NAME [--bgp-routing-mode=MODE; default="regional"] [--description=DESCRIPTION] [--range=RANGE] [--subnet-mode=MODE] [GCLOUD_WIDE_FLAG …]

        - NAVIGATION-MENU>>>VPC network>>>Create VPC network
        - Subnet creation mode>>>Custom
        - Firewall>>>Create firewall rule

# STEP 2 : Create the VM instance with no public IP address


# STEP 3 : SSH to vm-internal to test the IAP tunnel
        -Activate Cloud Shell
        -connect to vm-internal with the following command
            -gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
        -Test the external connectivity of vm-internal
            -ping -c 2 www.google.com
        -Exit and return to google cloud console.

# STEP 4:  Enable Private Google Access
        -By default, Private Google Access is disabled on a VPC network.

        -first; Create a Cloud Storage bucket
            ==> gsutil mb [-b (on|off)] [-c <class>] [-l <location>] [-p <proj_id>]
          [--retention <time>] gs://<bucket_name>
            -[Navigation menu>>>Storage >>>Browser>>>Create bucket] Google console steps to create a bucket.
        
        - Copy an image file into your bucket
            - gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://[my_bucket]

        -Refresh Bucket to verify that the image was copied in the google console.

        -click on the name of the image in the Cloud Console to view an example of how Private Google Access is implemented.

        - try to copy the image from your bucket, run the following command, replacing [my_bucket] with your bucket's name:
            - gsutil cp gs://[my_bucket]/*.svg .
        
        - connect to vm-internal
             - gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
        
        - try to copy the image to vm-internal, run the following command, replacing [my_bucket] with your bucket's name:
            - gsutil cp gs://[my_bucket]/*.svg 
        
        -  This should not work: vm-internal can only send traffic within the VPC network because Private Google Access is disabled (by default).

# STEP 5 : Enable Private Google Access
Private Google Access is enabled at the subnet level

    -Navigation menu>>>VPC network>>>VPC networks.
    - Open  privatenet-us
    - Edit and select on for Private Google access
    - Save and exit
  -In cloudshell for vm internal try and copy the image again. 
        -gsutil cp gs://[my_bucket]/*.svg 
        -This should work vm-internal's subnet has Private Google Access enabled

# STEP 6 : Configure a Cloud NAT gateway
    Navigation menu>>>Network services>>>Cloud NAT.
    Get started and specify the defaults as selected/wanted:
            -Gateway name == nat-config
            -VPC network == privatenet
            - Region ==	us-central1

    Create a new router under the cloud router
    
    Verify the Cloud NAT gateway
        -In Cloud Shell for vm-internal, run the following command
            -sudo apt-get update
        -This should work because vm-internal is using the NAT gateway
        
# STEP 7 : Configure and view logs with Cloud NAT Logging
    -Cloud NAT logging allows you to log NAT connections and errors

    -Enabling logging
        - Navigation menu>>>Network services>>> Cloud NAT
        - Select the Gateway created above and edit
        - Click the Logging, minimum ports, timeout dropdown to open that section.
        - Under Stackdriver logging, select Translation and errors and then click Save.

# STEP 8 : Generating logs
    -Cloud NAT logs are generated for the following sequences:
        -When a network connection using NAT is created.
        -When a packet is dropped because no port was available for NAT.