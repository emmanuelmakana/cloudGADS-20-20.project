# Getting Started with GKE

# OBJECTIVES
    1. Provision a Kubernetes cluster using Kubernetes Engine.
    2. Deploy and manage Docker containers using kubectl.

# STEP 1: ENABLE THE NEEDED APIS
On the API and SERVICES menu search for the apis and enable if not enabled.
    1. Kubernetes Engine API
    2. Container Registry API
   
# STEP 2: Start a Kubernetes Engine cluster
    1. Open the cloud command shell and place your specific zone to the environment  variable.
        --export MY_ZONE='YOURZONE'
    
    2. Start the kubernetes cluster managed by Kubernetes Engine and name it webfrontend and configure it to run 2 nodes.
        -- gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

    3. After install is complete check the installed version of the cluster using kubectl command
        -- kubectl version

# STEP 3: Run and Deploy a container
    1. From the cloud shell run and launch a single instance of a webserver
        --kubectl create deploy nginx --image=nginx:1.17.10
    2. View the created pod running the nginx server
        --kubectl get pods
    3. Expose the nginx to the internet
        --kubectl expose deployment nginx --port 80 --type LoadBalancer

            --Kubernetes created a service and an external load balancer with a public IP address attached to it.

    4. View the new service
        --kubectl get services

        ==> You can use the displayed external IP address to test and contact the nginx container remotely

    5. Open a browser window and paste the external ip address; the nginx webserver default homepage is displayed
    6. Scale up the number of pods running the service created to 3
        --kubectl scale deployment nginx --replicas 3
            --important when increasing an application thats becoming more popular.

    7. Confirm that the number of pods have been added.
        -- kubectl get pods
    8. Confirm that the external ip has not changed
        -- kubectl get services
    