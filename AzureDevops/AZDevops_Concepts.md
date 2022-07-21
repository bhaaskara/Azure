# What is Azure Devops
Azure DevOps provides developer services that allows organizations to create and improve products at a faster pace than they can with traditional software development approaches.

# Components of Azure Devops
- **Azure boards**
    - delivers a suite of Agile tools to support planning and tracking work, code defects, and issues using Kanban and Scrum methods.
    - Helps in planning, tracking and working with work items across teams.
- **Azure repos**
    - Managing your source code repositories
    - Provides Git repositories or Team foundation version control
- **Azure pipelines**
    - Provides build and release services to support continuous integration and delivery of your applications.
- **Azure artifacts**
    - This is used to manage your packages
    - allows teams to share packages such as Maven, npm, NuGet, and more from public and private sources and integrate package sharing into your pipelines.
- **Azure test plans**
    - provides several tools to test your apps, including manual/exploratory testing and continuous testing.

## Azure Boards
- Azure Boards is a service for managing the work for your software projects.
- Azure Boards brining you a rich set of capabilities including native support for Scrum and
   Kanban, customizable dashboards, and integrated reporting.
- There are mainly 5 core features to Azure Boards:

![](Pasted%20image%2020220525194727.png) 

**Work Item**
- work item is the artifact used to track any work on the azure board.
- each work item uses a state model to track and communicate progress.

![](Pasted%20image%2020220525195225.png)

**Boards**
- Each project comes with a pre-configured Kanban board perfect for managing the flow of your work
- Boards are highly customizable allowing you to add the columns you need for each team and project.

**Backlog**
backlogs help you keep the things in order of priority, and to understand relationship between your work.

**Sprints**
Sprints give you the ability to create increments of work for your team to accomplish together.

**Dashboard**
Azure dashboards comes with a rich canvas for creating dashboards. Add widgets as needed to track progress and directions.

## Azure pipe line
https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops

# Continuous Integration
```
In Continuous Integration we create the build pipeline
which get the code from GIT and build the code and publish the artifacts.
```
Bug fixes or feature developments on feature branches will be merged on to master/main branch through pull requests.

## Azure Pipelines
### Create a New pipeline
Azure Devops -> Project -> Pipelines -> Create/New pipeline
![](Pasted%20image%2020220716230328.png)

Select the Repo for your pipeline yaml
![](Pasted%20image%2020220716231424.png)
![](Pasted%20image%2020220716231456.png)
Note: Depending on the code in branch azure pipeline would suggest pipeline template types
          i.e .net, java or python and a new pipeline with suggested steps will get populated and you can modify it.
          
![](Pasted%20image%2020220716231519.png)

Sample pipeline code
```yml
pool: 
  vmImage: "ubuntu-latest"

steps:
- bash: echo "our first pipele"
```

![](Pasted%20image%2020220716231752.png)
![](Pasted%20image%2020220716231908.png)
![](Pasted%20image%2020220716231934.png)
![](Pasted%20image%2020220716232004.png)

![](Pasted%20image%2020220716232050.png)

To edit the pipeline yaml goto pipeline and click edit.

### Publish build artifacts
In pipeline yaml add `publish build artifacts` step
![](Pasted%20image%2020220717180634.png)
![](Pasted%20image%2020220717180713.png)
![](Pasted%20image%2020220717180756.png)

### Agent pool
- Microsoft hosted agents
- Self hosted agents

#### Self hosted agent pool
1. Create a VM in Azure
2. Install Azure pipe line agent
3. Install required tools
    1. Git
    2. .net
    3. python etc..

**1. Create a VM in Azure**
Create a VM with all default options and can be any region

**2. Install Azure pipeline agent**

Azure devops -> Project settings -> Pipelines -> Agent pools
![](Pasted%20image%2020220717120014.png)
Azure pipelines - Is used for Microsoft hosted agent pool.
Default - is used for self hosted agents

Add a node/VM to default pool
![](Pasted%20image%2020220717120154.png)
![](Pasted%20image%2020220717120221.png)
Use PAT (personal access token) to authenticate.
Create one in Azure devops -> account -> PAT -> with scope of agent read and manage access.

![](Pasted%20image%2020220717120609.png)

Use this agent in pipeline
```yml
pool
  name: 'default' 
  vmImage: 'agentvm' #AgentVMname
```

**Videos - 62-66 about Jenkins**

## Security in the CI/CD pipeline
![](Pasted%20image%2020220717134316.png)
**GAPS 68-81**

## Flaky test
A test is referred to as Flaky when it fails and was actually expected not to fail.
i.e When nothing is changed (the build (code), environment and test case) but still the test failed due to lot of jobs are running at the same time or due to some other reason.

Enabling a flaky test detection will identify the flaky test and make your build not to fail because of a flaky test.

**Enable a Flaky test**
Azure devops -> project -> settings -> test management -> Flaky test detection.

## Parallel jobs
Free tier doesn't have parallel jobs enabled.
Azure subscription be merged with Azure devops billing.

## DOUBTS
```
how to use variables in azure pipeline
how to use user inputs
how to use multi stage pipeline
how to deploy on to AKS - may cover in AKS course
how to use passwords in pipeline
```

# Continuous Delivery
![](Pasted%20image%2020220717144237.png)

Continuous Delivery - Here the pipeline should be able to deploy the application onto the production based environment 
Continuous Deployment - This is where the entire process from code commit to production is automated. 
Here as soon as the code changes are pushed to the code repository, the code changes are deployed onto production. 
Here the customers Will receive the improvements as soon as they are available.

```
In continuous delivery the Release pipeline get the artifacts from build pipeline and deploys on to infra. 
```

## Azure release pipeline - web app
1. Create Azure webapp on Azure cloud to deploy the .net app
2. Create the Azure release pipeline
    Azure devops -> Pipeline -> releases -> new pipeline
    ![](Pasted%20image%2020220717180919.png)
      ![](Pasted%20image%2020220717181124.png)
![](Pasted%20image%2020220717181238.png)
Add build pipeline for Artifacts
and click on ![](Pasted%20image%2020220717181420.png)
![](Pasted%20image%2020220717181649.png)
## Continuous Trigger
Azure devops -> Pipeline -> releases -> (release pipeline) -> edit
![](Pasted%20image%2020220717182021.png)

## Multiple Stages
**Parallel stage**
![](Pasted%20image%2020220717183054.png)
select template
![](Pasted%20image%2020220717183125.png)


**Sequential stage**
![](Pasted%20image%2020220717183243.png)
select the template
![](Pasted%20image%2020220717183314.png)
**GAPS - 124-125 Azure pipelines using ARM templates**

## Approvals and Gates
Approval and gates can give more control over the deployment pipeline. 
Here you can define pre-deployment and post-deployment conditions in the pipeline. 

**Scenarios** 
Let's say that a user needs to validate a change request before the deployment  can be done to a Stage — Pre-deployment approval. 
Let's say that a user needs to validate the deployment which has been done to a stage before the deployment can be done onto another stage - Post-deployment approval
Sometimes you might want to ensure that there are no active issues for the project before the deployment is carried onto a stage - Pre-deployment gate. 
If an application has been deployed onto a staging environment and it needs to 
ensured that no incidents are recorded in the incident management system 
before the application is deployed onto the production environment - Post- 
deployment gates 

**Add Approvals**
![](Pasted%20image%2020220717191509.png)

![](Pasted%20image%2020220717191613.png)

**Setup Gates**
![](Pasted%20image%2020220717191509.png)

![](Pasted%20image%2020220717192332.png)

We can select **Azure policy as gate**

Query for a bug which is not closed
![](Pasted%20image%2020220717192554.png)
save this query as shared queries
![](Pasted%20image%2020220717192751.png)
Give permission for your project for the shared queries
![](Pasted%20image%2020220717192959.png)
More options -> security
![](Pasted%20image%2020220717193114.png)
![](Pasted%20image%2020220717193143.png)

**GAPS 132 - 170**

## Azure Build pipeline - Deploy on to AKS
1. Upload the deployment.yml and service.yml to the azure repo or Github
` check it in AKS course`

## Azure Key Vault

## Azure pipeline variable group
1. Create a variable group in Azure pipeline -> library -> variable group
2. Accessing it in Azure pipeline
```yml
# Docker
# Build and push an image to Azure Container Registry
# https://docs.microsoft.com/azure/devops/pipelines/languages/docker
 
trigger:
- master
 
pool:
  vmImage: 'ubuntu-latest'
 
variables:
- group: demogroup
stages:
- stage: demostage
  jobs:
  - job: Test
    steps:
    - script: echo $(secret)
```

## Trouble shooting Pipeline



# Deployment strategies
1. Blue green deployment
    ![](Pasted%20image%2020220718122108.png)
2. Canary deployment
    ![](Pasted%20image%2020220718121951.png)
1. Rolling deployment
2. Feature flags


## Ref
**Azure pipeline tutorial**
https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops

**Stages in Azure pipeline**
https://docs.microsoft.com/en-us/azure/devops/pipelines/process/stages?view=azure-devops&tabs=yaml

**YAML schema reference for Azure Pipelines**
https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines

**Microsoft hosted agents**
https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml

# Azure Repos
with Azure Repos you can Git or Team foundation version control system.

# Azure Artifacts
This service allows you to create and share Maven(java), npm(node JS) and NuGet(.net) package 
feeds from public and private sources. 
In Azure Artifacts , the packages are stored in feeds. 
With feeds you can group packages and control the access to the packages. 
You can create public feeds from within an Azure DevOps project. 
These feeds allow one to share packages publicly with anyone on the Internet

## Create Feed
Azure devops -> project -> Artifacts -> Create new feed
![](Pasted%20image%2020220719174103.png)
Once created connect to feed

## Publish packages to the Azure Artifacts
Packages in Azure artifacts are published and consumed through feeds, so we have to publish to feed.

```sh
1.  nuget sources list

2.  nuget sources add -Name demofeed -Source "https://pkgs.dev.azure.com/techsup1000/demo-project/_packaging/demofeed/nuget/v3/index.json" -username feed -password nu2lhnscukzokiqa7hbtb3wzpwmkzid4xfj4mru3cbkkazccejwa

3.  nuget push -Source "demofeed" -ApiKey az demolibrary2000.1.0.0.nupkg
```
Note: you can get this info from Azure Artifacts -> feed -> connect to feed

**How to get access token**
![](Pasted%20image%2020220719200536.png)
![](Pasted%20image%2020220719200641.png)
