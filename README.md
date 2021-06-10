### Demo Repo - CI/CD and docker

#### This repo contains 3 Elements
1. At root level based on spring-boot serving-web-content project ( [tutorial](https://spring.io/guides/gs/serving-web-content/) ) 
    - Maven project - build using `mvn clean package`
    - Dockerfile - Packaging this project in Image  
    - Jenkinsfile - CI/CD for this project  

2. deployment folder 
   - containing ansible playbook and 3 roles to deploy the serving-web-content 
      project docker image to a server
   - Dockerfile (centos-7 with ansible) for deploy stage 

3. Jenkins-Master folder - resources
   - private-key for ssh to the aws instances
   - list of ip - first is the Jenkins_Master, second for deployment
   - docker-compose file /var/lib/jenkins it's a good-to-go `docker-compose.yaml` (doesn't need any changes)
    
#### Status
1. Jenkins server is running on docker working successfully but after restart needs manual start.
2. The serving-web-content Project Image is also builds correctly and tested locally.
3. Ansible's roles also validated and working.
4. Jenkinsfile was tested in a previous environment but in the latest cyberattack  
   the repo has been deleted. The only backup we found has the previous commit 
   
### Since you are our last hope we giving you this backup file and counting on you to save the day

### Your mission, should  you choose to accept it
#### Complete the following tasks which are also referenced as _TODO_ tasks on the jenkins file 
1. ***Jenkins_Master***
   - Enable that whenever it's restarting the docker engine and jenkins will start also
   (write the steps you did in the final email)
   
2. ***VCS***
   1. Deploy this content (source code) to an online VCS repo of your choice (GitHub/bitbucket/gitlab all has free accounts)
   2. Create develop, and release branches, so we can implement our gitflow methodology
   3. Connect GitHub (the vcs repo you choose) to Jenkins_Master via webhook trigger so each commit will trigger the build  

3. Docker ***Image*** build 
   - We don't have docker registry and not alloed to put our private app on docker hub so
   - In the Jenkins file at `stage("prepare image to deploy")` Add an `sh ""` (shell command) to save the image as a tar file in the deployment folder
   
4. ***Deployment*** with Ansible
   1. Add SSH user & key credentials object in Jenkins_Master for ansible to connect to the deployment instance
   2. Put the deployment server IP in the inventory file 
   3. Set the image param for the ansible process with the image value.

##### Resources:
***In the Jenkins-Master folder***
1. jkey - the private key for servers connect with default ec2 user
2. ips - list of ips to use with the key one for Jenkins and one for deployment
3. jAdmin - user pass to jenkins UI

### The environment will self-destruct at ... 
