# kubernetes-deployment-through-jenkins

CREATE PUBLIC GKE CLUSTER:

    go to kubernetes engine > clusters > create new cluster 
    
    cluster basics: 
          name:
          zonal cluster: 
          zone: asia-south1-a
          rest is default
    default pool:
          nodes: 
                series: e2
                machine type: e2-medium 2vCPU 4GB memory (change this according to need)
                boot disc type: standard persistent disc
                boot disc type: 30GB
                select number of nodes: 1
          (leave networking, security and metadata as default)
    automation:
          keep everything default
    networking:
          network: default
          node subnet: default
          network access: public cluster
          enable vpc-native traffic routing
          enable http load balancing
    (leave security, metadata and features as default)
    
    
    EQUIVALENT COMMAND TO CREATE PUBLIC CLUSTER: (make changes according to the need)
          
          gcloud beta container --project "sinuous-axiom-365214" clusters create "demo" --zone "asia-south1-a" --no-enable-basic-auth --cluster-version "1.23.12-gke.100" --release-channel "regular" --machine-type "e2-small" --image-type "COS_CONTAINERD" --disk-type "pd-standard" --disk-size "30" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --max-pods-per-node "110" --num-nodes=1 --logging=SYSTEM,WORKLOAD --monitoring=SYSTEM --enable-ip-alias --network "projects/sinuous-axiom-365214/global/networks/default" --subnetwork "projects/sinuous-axiom-365214/regions/asia-south1/subnetworks/default" --no-enable-intra-node-visibility --default-max-pods-per-node "110" --no-enable-master-authorized-networks --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --enable-shielded-nodes --node-locations "asia-south1-a"
          

CREATE JENKINS CUSTOMIZED IMAGE, PUSH TO DOCKERHUB AND ACCESS

    this image consists of kubectl and docker installed in it. 
    link to github repo from where dockerfile can be obtained: https://github.com/cirolini/jenkins-docker-kubectl.git
    (comment plugins part in the dockerfile and just keep Docker and Kubectl)
    
    command to build image: docker build -t <image name with tag> .
    
    command to push image to docker: docker push <image name with tag>
    
    command to run jenkins container: docker run --name jenkins -p 8080:8080 -p 50000:50000 -d -v C:/Users/ashishkulkarni/Desktop/Jenkins_volume -v           /var/run/docker.sock:/var/run/docker.sock ashishdocker0/jenkins:1.0
    (change image name and remove volumes if needed)
    

CONFIGURING JENKINS WITH GKE:
    
    manage jenkins > manage plugins 
        search 'kubernetes' in available section.
        
    manage jenkins > manage credentials 
        create credentials : create new credentials
            kind: private key from service account (give name and upload json file)
        
        create credentials for github if needed
            kind: username and password
            
    manage jenkins > configure nodes and cloud > configure clouds
         selet cloud: kubernetes
         name: kubernetes 
         kubernetes URL: type command inside cluser: 'kubectl cluster-info' copy kubernetes control plane url (example https://<control plane ip>).
         disable https certificate check
         credentials: add credentials of kubernetes which has private key of service account
         jenkins URL: http://localhost:8080
         now test the connection
         and hit save button.
         
         
CREATE JENKINS PIPELINE AND CREATE JWNKINSFILE:    
     
         
            
            
        
    
    
    
    
    
