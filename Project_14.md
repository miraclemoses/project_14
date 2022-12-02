# Project 14 - Experience Continuous Integration With Jenkins | Ansible | Artifactory | Sonarqube | PHP

## Synopsis
Simulating a typical CI/CD pipeline for a PHP based application

As part of the ongoing infrastructure development with Ansible started from [project 11](https://github.com/miraclemoses/darey.io-projects/tree/main/project_11), you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both Tooling and TODO Web Applications are based on an interpreted (scripting) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible uri module.  

![](./CI_CD-Pipeline-For-PHP-ToDo-Application.png)

#### Setup
The final setup to be simulated.  

![](./Environment-setup.png)


### 1. Configuring Ansible For Jenkins Deployment
- Created new Jenkins-Ansible server
- Installed "Blue Ocean" plugin
- Connected Jenkins to github using generated access token
- Created new pipeline to existing [ansible-config-mgt](https://github.com/miraclemoses/ansible-config-mgt) repo used in [project 13](https://github.com/miraclemoses/darey.io-projects/tree/main/project_13)
- Added and updated `Jenkinsfile` to contain simple stages with echo commands
- Committed and pushed
- Triggered build on Jenkins  

![](./simple-jenkinsfile-main.png)


### 2. Run Ansible Playbook from jenkins
* Installed the "Ansible" plugin on jenkins
* Added private ssh key to global credentials on jenkins
* Configured jenkins to run ansible playbook through implementation of stages/steps in `Jenkinsfile`
  - Used the jenkins default `BRANCH_NAME` environment variable in the `checkout SCM` stage to ensure true and dynamic multibranch build based on push. 
  - Ensured deploy environment inventory file was parametarized for ansible
* ssh-ed into nginx and mysql servers to confirm successful installation

See images below:

**2a. Parameterized build**  

![](./deploy-env-parameter.png)

**2b. Dynamic Multibranch build for `dev` and `main` without modifying Jenkinsfile**  

![](./dev-pipeline-ansible.png)  

![](./main-pipeline-ansible.png)

**2c. Successful Installations for `nginx` and `mysql` servers**  

![](./nginx-installed.png)  

![](./mysql-installed.png)


### 3. CI/CD pipeline for Todo application

#### 3.1 Installed artifactory using ansible on artifactory server
- Created artifactory server
- installed artifactory plugin on jenkins server
- Implemented ansible artifactory role  

![](./artifactory-installed.png)

#### 3.2 Integrate Artifactory repository with Jenkins
- Implemented update via ansible to create homestead db and user on db server
- confirmed db and user is created  

![](./homestead-db-created.png)
![](./homestead-confirm.png)

#### 3.3 Code Quality Analysis
- Implemented php todo app jenkins pipeline
- installed plot plugin on jenkins  

![](./php-todo-first-pipeline.png)
![](./plot-jenkins.png)

#### 3.4 Bundle, Publish and Deploy php todo app
- Implemented stage to trigger ansible-config-mgt job after successfully running php-todo job
- Launched todo webserver instance on AWS
- Update ansible-config-mgt job to deploy php-todo application from artifactory to todo-webserver  

![](./todo-delpoyment-ansbile.png)
![](./todo-deployment-artifactory.png)

### 4. Sonarqube Installation
- Implemented sonarqube role on ansible and ran via jenkins server  

![](./sonarqube-ansible-setup.png)

- Sonarqube installed and running successfully  

![](./sonarqube-installed.png)

### 5. Configure Sonarqube and Jenkins for Quality Gate
- Added quality gate check to jenkins pipeline  

#### 5.1 Update Jenkins Pipeline to include SonarQube scanning and Quality Gate
- Successful quality at first run  

![](./quality-gate-first-run.png)

- Failed quality gate check after scanner setup  

![](./failed-quality-gate.png)

- Display tech debt on sonarqube  

![](./php-todo-tech-debt.png)

- Setup Quality Gate to deny deployment when code quality check fails  
![](./no-deploy.png)

Link to video showcasing jenkins pipeline and sonarqube (on youtube)[https://www.youtube.com/w]
