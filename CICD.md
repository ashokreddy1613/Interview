### If we have 5 stages in a jenkins pipeline and 5th stage having syntax error then what will happen if we run the pipeline.

The pipeline will be failed even before any stage is executed, because Jenkins parse the whole Jenkins file at the start,syntax errors caught during parse phrase, not at runtime.

### What are some main differences between scripted and declarative pipeline.
The main difference between Declarative and Scripted pipelines is in their syntax and flexibility.

Declarative pipilines use a structured , easy to write, read, and validate. 

Scripted pipelines are written in `groovy` language, provide more control and flexibility, which is useful for more complex logic and dynamic behaviour. 

### Difference between code quality and code coverage.
Code quality refers to how clean, maintainable, and bug-free the code is. It’s usually measured through static analysis tools like SonarQube, which detect issues such as code smells, complexity, or security vulnerabilities

Code coverage, on the other hand, measures how much of the codebase is exercised by automated tests. Tools like JaCoCo or Istanbul help calculate what percentage of code is tested.

### What is default qualitygate in sonar
The default qualitygate in soanr is called `sonarway`
A quality gate is a set of conditions that a project must meet inorder to be considered as have passed code quality standard.
There are default conditions in `sonar way` quality gate.

which includes conditions like at least 80% code coverage on new code, no critical bugs or vulnerabilities, and no more than 3% duplicated code.

### What is git branching strategy used in your organisation
### How deployment is done in different environment using git repo

### How and from where to clone repo, is there any local repo you are using and then transferring from local to remote or how? 
In my workflow, I usually clone repositories directly from a remote Git server such as GitHub, GitLab, or Azure Repos, depending on the organization.

### What is PAT
PAT (Personal Access Token) is a secure, token-based alternative to passwords used to authenticate with Git hosting platforms like: GitHub,GitLab etc

### How to configure sonarqube
There are couple of things to configure SonarQube
    1. Download and install sonarqube then start it
       Access it on localhost:9000, generate token
    2. Install SonarQube plugin in Jenkins
       Configure SonarQube in Jenkins
        -configure system-
        add sonarQube and provide Name, Server URL, sonar token 
        then create sonar step in jenkins pipeline

### How to handle merge conflict in git. If 2 people working on same file and did the commit and got conflict err, in how many ways it can be solved.
Merge confilcts occurs when git can't automatically resolve changes to the same part of file. I usaullly resolve them manually or using a merge tool, and then staging and commiting the resolve file.

There are multiple ways to resolve:
    Keep current (ours)
    Keep incoming (theirs)
    Merge both manually
    Use a visual merge tool

### What is the output of sonarqube, how to fix if any smell code/vurnabilities found
SonarQube outputs a detailed report with metrics like bugs, code smell, vulnerabilities, duplicaitions, code coverages. It also provides quality gate status that determines if the code meets predefined quality standards.

When SonarQube flags code smells or vulnerabilities, I examine the specific rule that triggered it, refactor the code accordingly (e.g., modularizing long methods, removing hardcoded secrets, fixing security flaws), and then re-run the analysis to confirm the issue is resolved. This workflow helps ensure high-quality, maintainable, and secure code before deployment.”

### Where do you write the code/yaml file for pipeline
I write our pipeline code in the root of the Git repository, using a `Jenkinsfile` for Jenkins or .yml files for GitLab/GitHub/Azure

### how to schedule pipeline, lets say i have validated the pipeline with some update and i want to schedule it to stage/main branch, how to do? 
In Jenkins, I schedule pipelines using the `cron` trigger inside the jenkinsfile. After validaitng feature branch, we use scheduled jobs to promote to staging or main branch.

For example, we might set a job to run every night at 2 AM using cron('H 2 * * *'), pulling from the main branch and deploying it to staging.

### In Git, explain the push and pull commands.
Push: use to push code to remote repository
Pull: Download or pull from remote repo to local development env

### What is the use of Git tags?
1. Marking Releases
Git tags are commonly used to mark stable versions of code.
2. Versioning
Helps associate a Git commit with a semantic version 

Git tags are used to mark specific commits, typically for versioned releases like v1.0.0. Unlike branches, tags don’t change and are ideal for pointing to stable snapshots of code.

We use tags in our CI/CD pipelines to trigger deployments, track production versions, and simplify rollbacks. I prefer annotated tags for releases since they include metadata like author and message.

### What are the different types of branches in Git?
Usaully, long live branches and short live branches
Long live branches are Master/ main and develop
Short live branches are feature, hotfixes,

### What kind of CI/CD pipelines are you familiar with?
I am familiar with couple of pipelines but I have been wide working on Jenkins

### Whats ur organisation current cicd process and tools

### What securities measures/tools u were taken in ur cicd pipeline?
Security is most important for any application or organization, I do take it as one of the top priority whenever I build or create something, could be in cloud or outside of cloud.

Talking of CICD, I enforced many security measures
firstly, Credential & secret management:
    We use Jenkins secret credentials to store the API keys, password, and never store plain text secrets.
2. SAST(Static code analysis)
    Integrated with Jenkins pipeline to scan for:
        Code quality issues, vulnarabilities
3. Image scaning
    use trivy to scan all the docker images before push ECR or DockerHub
4. Access Control
    Implememts RBAC, provide least privilege priciples 

### how do you set approval in cd pipeline
 Approvals in a CD pipeline are used to pause the pipeline at critical stages (usually before deploying to production), requiring a manual review or sign-off by an authorized user or team.
They help ensure compliance, security, and change control before code goes live.

Jenkins – Input Step in Declarative Pipeline
```bash
stage('Approval') {
  steps {
    input message: 'Deploy to production?', ok: 'Approve'
  }
}
```
 ### what are the branching strategies you use-notAnswered
### CI/CD pipeline needs rollback capability. How would you implement it?
### Jenkins is failing to push a Docker image to the registry. How do you troubleshoot?
How would trigger pipeline B in jenkins automatically after pipeline B
Explain how you would set up a multi-branch Jenkins pipeline for a GitHub repository.
 How would you implement dynamic stages in a Jenkinsfile based on environment variables? 
 A Jenkins pipeline is randomly failing at the deployment stage to EKS. Logs show timeouts during kubectl apply.
 How will you check the vulnerability of your code
 pls explain the flow how the pipeline will trigger across different environments 