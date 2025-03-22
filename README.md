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
