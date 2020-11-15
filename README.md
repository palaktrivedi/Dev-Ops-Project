# Edureka Project Certificate for DevOps submitted by Palak Trivedi	

## Batch: Oct 2020

## Link: 

###  Checklist for Jobs

A. CI CD Tool (Jenkins)

1. Install and configure puppet agent on the slave node (Job 1) 

2. Sign the puppet certificate on master using Jenkins (Job 2)

3. Trigger the puppet agent on test server to install docker (Job 3)

4. Pull the PHP website, Dockerfile and Selenium JAR from your git repo and build and deploy
your PHP docker container. After this test the deployment using Selenium JAR file. (Job 4)

5. If Job 4 fails, delete the running container on Test Server

### Steps for executing the project

a. Setup connection on Jenkins with Master node and Slave Node. It includes creating a slave node through master node Jenkins Dashboard.
b. Setup Puppet on Slave node. The host files need to be updated with the master and slave IPs. Connect Puppet with Master VM. 
c. Sign the certificate of Slave VM on Master VM
```
puppet cert list --all
puppet cert sign --all
```
d. Create a puppet file to install docker with content as the one in the repository (init.pp). 
```
pdk new module install-docker
cd install-docker/manifests 
vim init.pp
pdk build  install-docker
opt/puppetlabs/bin/puppet module install /home/edureka/modules/install-docker/pkg/filename.tar.gz
```
e. In the slave VM, execute  
`/opt/puppetlabs/bin/puppet agent -t `
f. Ansible playbooks need to be executed on with the Master VM. The hosts file needs to be appended with the IP of the host VM. The contents of the file are in chrome_playbook.yml and git_playbook.yml.
```
ansible-playbook chrome_playbook.yml
ansible-playbook git_playbook.yml
``` 
g. Jenkins Pipeline needs to be created with the jobs interconnected. The pipeline will execute on Slave VM. The first job of Jenkins instructs to pull the repository and build the DockerFile. So, the Github link is provided along with credentials in SCM section. And in build section, the Dockerfile is built. 
h. Second Job instructs to run the test.jar file. It should give a Fail result when run. The job has been setup such that when it fails it will move on to execute third job. 
i. As the Second job failed, the third job will run which is instructed to stop the running container. The docker plugin in Jenkins allows to start/stop the running container. Afer stopping, the container is deleted.   
