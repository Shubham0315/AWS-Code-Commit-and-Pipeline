# AWS-Code-Commit-and-Pipeline
Illustrates the AWS services to implement CICD. 

- Here we'll use AWS managed services to implement CICD on AWS.

- We generally use GitLab CI, GitHub Actions, Jenkins CI to implement CICD on on-premises or cloud setup.
- AWS also has its set of managed services such as AWS Code commit, AWS Code pipeline, AWS Code build, AWS Code deploy using which we can implement CICD on AWS

- Suppose we've application needs to be deployed on kubernetes or EC2 and we're using jenkins as orchestrator
  - We need place where code is hosted like GitHub  
  - When someone pushes/commits code, it triggers webhook configured which triggers pipeline in Jenkins  
  - Within pipeline we have build process which is done by docker/maven 
  - Take that built image and deploy it on our server(K8S/EC2)  --> argo cd, shell script
 
- AWS instead of using 4 different tools for 4 stages which are open source, AWS implements the flow with its own services
  - For VCS, hosting code(GitHub) :- AWS Code commit
  - To build pipeline (Jenkins) :- AWS Code pipeline
  - To build app (maven) :- AWS Code build
  - To deploy app (shell script) :- AWS Code deploy
 
- AWS provides set of CICD services that enable developers to automate and streamline their software delivery process
- AWS Code pipeline, AWS Code build, AWS Code deploy are the key services involved in achieving CICD on AWS platform itself

-------------------------------------------------------------------------------------------

AWS Code Commit
- 
- It solved problem of VCS
- It is similar to GITHUB/GITLAB but managed by AWS
- We use GitHub/GitLab for personal projects, storing open source projects, contributing to open source projects. But when we work in organization, we dont use public GitHub repo. Here we can use GitLab and use private repo or use GitHub enterprise version
- Organizations use GitHub but they use private GitHub repos and many organizations also use hosted git solutions. Either they use gitlab package and use it on their own servers (EC2 or on-premises)

- In AWS, Instead of downloading GITLAB and installing on our organization'servers or on premises servers and scale as required, AWS comes with managed solution for this in form of AWS CodeCommit
  - It is a managed Git Service which is scalable and reliable
  - Scalable as if our organization grows or we scale the managed servers/apps in number or we increase managed GIT servers, we have to patch/secure them timely. So AWS comes with code commit. We can keep creating repos, AWS will scale itself we just need to pay
  - Also its reliable as its AWS solution we can reach out to them
 
- In organization no one use free github repo


Practical
-
- AWS Code commit doesnt work well with root user due to some access restrictions, SSH or HTTPS, so switch to IAM account
- Go to code commit - Create repo- Provide name - Description(optional)
  - No one can access the repos unless they're part of our organization as they're private. In GitHub we can make them public if required.

![image](https://github.com/user-attachments/assets/f3c5dbaf-2220-4a91-8ec0-39bab6116c02)
![image](https://github.com/user-attachments/assets/07e2c77e-6bac-4d5b-b587-1df409d631f9)

- After creating repo, we can add/upload files from UI features like pull requests, commits, branches, etc using git commands
  - But here uploading/editing files can be done one by one using UI
  - To edit/upload multiple files as part of our code/terraform scripts, we need terminal
  - Even to clone repo on terminal, we cannot use root account
 
- To access AWS code repo from your laptop terminal we cannot use root account, ensure GIT is installed
  - Login using IAM user and clone URL using HTTPS :- git clone $URL

- Create IAM user now and attach policies directly - **AWSCodeCommitPowerUser** (to get all access of codeCommit)

![image](https://github.com/user-attachments/assets/59f0fe51-ded6-40bb-a4d7-bfd34973cb31)
![image](https://github.com/user-attachments/assets/c82a7c0b-8e32-47c7-9bff-e12342624378)

  - Using this user we can HTTPs into code commit and upload files in terminal

- Login through IAM user using incognito to access the repo created earlier (create it first we haven't)

- To access the Code-Commit repo using local terminal, we need to install GIT on local

- Command :- **git --version**

- Clone the URL :- **git clone https://URL**

- Once cloned we can create files - git add - git commit -m "message" - git push

- Disadvantages of Code Commit
  - Very less features than GitHub/GitLab
  - Restricted to only AWS
  - Even organizations using AWS host their repos on GitHub
  - Has less integrations with services outside AWS

-------------------------------------------------------------------------------------------

AWS Code-Pipeline
-
- Jenkins is an orchestrator which is open source tool
- Suppose our code is on GitHub and needs to be deployed on K8S.
  - User will commit code in GitHub. Devops engineer will create webhook that triggers jenkins pipeline(declarative/scripted)
  - These pipelines are responsible for 2 actions :- Implement CI and to invoke C-Del (Jenkins is CI platform only but it can invoke CDel)
  - In CI we've stages of pipeline such as checkout, build, test, code scan, image build, image push,etc
  - Once image us built ande pushed, jenkins will invoke C-del process (Ansible, shell script, argo cd). GitOps is recommended here as we not only push artifacts on cluster but we can also manage them.
  - Here Jenkins acts as orchestrator.
 
- Workflow of Code Pipeline
  - User makes commit/code change to AWS code commit
  - AWS Code commit after change triggers AWS Code pipeline which invokes CI and CD both (In jenkins it was implement CI and invoke CD)
  - It invokes CI using AWS Code build and implement all stages of CI like checkout, build, code scan, build, push, etc
  - Then code deploy takes care of CD to deploy code on K8S clusters or EC2
  - Thus AWS Code pipeline acts as orchestrator like jenkins
 
- AWS Code commit doesnt offer as many features like GitHub or GitLab. So developers instead of using Code Commit use GitHUb/GitLab

- If Code Commit/pipeline is chargeable and open source solutions like Jenkins are available, why people use AWS managed services?
  - Jenkins is open source with master-slave architecture. If we use 2 worker nodes for 50 jenkins pipelines initially and then the number increases. Here we've to increase no of worker nodes and manage all of them by ourself. We need to do things like managing, scaling, security patching, health check, etc.
  - There has to be one dedicated person to manage all those things
  - AWS here started their managed service like code services and AWS will charge us according to usage. This is done by organizations as they dont want to manage persons.
 
- Jenkins is preferred as it is Open source and for organizations management is not big problem.
  - Some companies can run their Jenkins workload on 2 EC2 instances.
  - When pipeline runs, we can setup like docker agent gets started and after completion it gets deleted so as to manage the infrastructure.
  - So managing AWS services is not preferred
  - Also we can create jenkins on EC2 and configure auto scaling groups, AWS cloudwatch, alarms.
 
** Why Jenkins in Popular than Code pipeline?
- Faetures of Jenkins and can be integrated with lot of tools
- Jenkins is not restricted to AWS platform only
