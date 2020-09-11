# Getting Started with Deployment Manager and Cloud Monitoring

# OBJECTIVES
    1. Create a Deployment Manager deployment.
    2. Update a Deployment Manager deployment.
    3. View the load on a VM instance using Cloud Monitoring.

# STEP 1 : Sign into GCP
    1. Sign into Google console platform or use the sdk

# STEP 2 : Confirm all the required APIs
    1. Confirm all the required APIs are enabled
        -Cloud Deployment Manager v2 API
        -Cloud Runtime Configuration API
        -Cloud Monitoring API
    -If any of the above API is missing enable

# STEP 3 : Create a Deployment Manager deployment
    1. Start cloud shell and set the required zone to an environment variable
        -export MY_ZONE=us-central1-a

    2. Download an editable cloud deployement manager template to use
        -gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml
    
    3. Use the sed(stream editor) command to replace the project_id placeholder string with the project_ID assigned/in use in the template
        -sed -i -e "s/PROJECT_ID/$DEVSHELL_PROJECT_ID/" mydeploy.yaml
    
    4. Use the sed(stream editor) command to replace the zone placeholder string with the zone assigned/in use
        -sed -i -e "s/ZONE/$MY_ZONE/" mydeploy.yaml

    5. View the yaml file and confirm all the changes have been changed as wanted
        -cat mydeploy.yaml
    
    6. Build a deployment from the template
        -gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml

    7. Confirm that the deployment was successful via the GCP CONSOLE
    8. Open the vm details and confirm that the startup script is included in the metadata.

# STEP 4: Update a Deployment Manager deployment
    1. Launch the nano text editor to edit the mydeploy.yaml file
        -nano mydeploy.yaml

    2. Find the line that sets the value of the startup script, value: "apt-get update", and edit it so that it looks like this
        - value: "apt-get update; apt-get install nginx-light -y"
    
    3. Save and exit the nano editor
        -CTRL + O  --> SAVE
        -CTRL + X  --> EXIT
    
    4. Open the cloud shell and restart the deployment manager to install the new start up script.
        -gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml
    
    5. Open the vm details and confirm that the startup script is included in the metadata and has been updated to the new one.
   

# STEP 5 : View the Load on a VM using Cloud Monitoring

    1. Navigate to the vm of you created and ssh into it.
    2. In the ssh session on my-vm, execute this command to create a CPU load
        -dd if=/dev/urandom | gzip -9 >> /dev/null &
    3. Create a monitoring workspace
        -In the GCP navigation menu find the Monitoring workspace and wait for it to be provisioned
        -Confirm that the GCP project ID is still the one on use.

    4. On the settings page access the AGENT tab and copy the code displayed to use on the VMs open ssh.
    5. install both the Monitoring and Logging agents on your project's VM using the code.
    6. After all the agents have been installed click the Metrics tab and study the resulting graphs,