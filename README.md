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
   - jkey - the private key for servers connect with default ec2 user
   - ips - list of ips to use with the key one for Jenkins and one for deployment
   - jAdmin - user pass to jenkins UI



### Status
1. Jenkins server is running on docker working successfully but after restart needs manual start.
2. The serving-web-content Project Image is also builds correctly and tested locally.
3. Ansible's roles also validated and working.
4. Jenkinsfile was tested in a previous environment but in the latest cyberattack  
   the repo has been deleted. The only backup we found has the previous commit 
   


### Since you are our last hope we giving you this backup file and counting on you to save the day...



### Since you are our last hope we giving you this backup file and counting on you to save the day...
### what i had to do: 

1. ***Jenkins_Master***
   - Enable that whenever it's restarting the docker engine and jenkins will start also
   
   
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

## when the program run: 

      the jenkins job is triggered to run when someone do commit to new change in develop branch, 
      such that we check if the change is accepted and create new pull so github trigger jenkins job 
      to run jenkinsfile and test the code. after that if we get that every thing is okey , we merge between release and develop branches.

















