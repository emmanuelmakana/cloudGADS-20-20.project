# LAB: GCP Fundamentals: Getting Started with Compute Engine

### OBJECTIVES

In this lab, we perfom the following tasks:
    -Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
    -Create a Compute Engine virtual machine using the gcloud command-line interface.
    -Connect between the two instances.

# Steps: 

1. Create a virtual machine using the GCP Console

    gcloud beta compute --project="PROJECT_NAME" instances create my-vm-1 --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=224117826907-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 --reservation-affinity=any


2. Create a virtual machine using the gcloud command line
   1. To display a list of all zones in the region run the command below and a choose any suitable zone.

    --gcloud compute zones list | grep us-central1

   2. Create a virtual machine in the selected zone.
    --gcloud config set compute/zone us-central1-b

    --gcloud compute instances create "my-vm-2"  --machine-type "n1-standard-1"  --image-project "debian-cloud"  --image "debian-9-stretch-v20190213"  --subnet "default"

3. Connect between VM instances

  1. open a command prompt on the my-vm-2 instance, click SSH in its row in the VM instances list
    -- Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network
    --ping my-vm-1

  2. Use the ssh command to open a command prompt on my-vm-1
   
        --ssh my-vm-1

  3. At the command prompt on my-vm-1, install the Nginx web server:
    --sudo apt-get install nginx-light -y

  4. Use the nano text editor to add a custom message to the home page of the web server:
    --Replace YOUR_NAME with your name:
    --sudo nano /var/www/html/index.nginx-debian.html

  5. Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.
  6. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:
    --curl http://localhost/
    --The response will be the HTML source of the web server's home page, including your line of custom text.
    --exit the command prompt on my-vm-1 with the EXIT command
    --return to the command prompt on my-vm-2

  7. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
    --curl http://my-vm-1/

4. Copy the External IP address for my-vm-1 and paste it into the address bar of a new browser.
    --gcloud compute instances list
    
5. End the lab.
     
    



    


