# What is Azure DevOps
Azure DevOps provides developer services that allows organizations to create and improve products at a faster pace than they can with traditional software development approaches.

# Components of Azure DevOps
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

# Organization / Project
Organization is collection of related projects.
All the azure DevOps components will be under the project.

Once you signup or sign in with your azure credentials to Azure DevOps you get an default organization and you can create organizations too.
Use your work or school account to _automatically connect_ your organization to your Azure Active Directory (Azure AD).
All organizations must be manually created via the web portal.

# Azure Boards
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

# Azure pipeline
https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops
https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/key-pipelines-concepts?view=azure-devops


![](Pasted%20image%2020220723001823.png)

![](Pasted%20image%2020220723002012.png)

Example Pipeline
```yml
# ASP.NET Core (.NET Framework)
# Build and test ASP.NET Core projects targeting the full .NET Framework.
# Add steps that publish symbols, save build artifacts, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core

trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'

steps:
- task: NuGetToolInstaller@1

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    solution: '$(solution)'
    msbuildArgs: '/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:DesktopBuildPackageLocation="$(build.artifactStagingDirectory)\WebApp.zip" /p:DeployIisAppPath="Default Web Site"'
    platform: '$(buildPlatform)'
    configuration: '$(buildConfiguration)'
```

# Continuous Integration (Build Pipeline)
```
In Continuous Integration we create the build pipeline
which get the code from GIT, build the code and publish the artifacts.
```

**Continuous Integration** means automatically building and testing the code every time a team member commits the code changes to version control.
Bug fixes or feature developments on feature branches will be merged on to master/main branch through pull requests.

## Create a New Build pipeline
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

To edit the pipeline yaml, go to pipeline and click edit.

### Publish build artifacts
In pipeline yaml add `publish build artifacts` step
![](Pasted%20image%2020220717180634.png)
![](Pasted%20image%2020220717180713.png)
![](Pasted%20image%2020220717180756.png)

## Agent pool
- Microsoft hosted agents
    - https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml
- Self hosted agents

### Self hosted agent pool
1. Create a VM in Azure
2. Install Azure pipe line agent
3. Install required tools
    1. Git
    2. .net
    3. python etc..
4. Register the agent

**1. Create a VM in Azure**
    Create a VM with all default options and can be any region.

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

**4. Register the agent**
on the VM
![](Pasted%20image%2020220817115358.png)
Note: All the build data will be saved in `C:\work` directory.
`C:\work\1\s` - Source code
`C:\work\1\a` - Archive files/builds

Use this agent in pipeline
```yml
pool
  name: 'default' 
  #vmImage: 'agentvm' #AgentVMname 
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

Check the parallel jobs settings
Azure DevOps -> Organization -> Org settings -> pipelines -> parallel jobs
![](Pasted%20image%2020220817201527.png)

## DOUBTS
```
how to use variables in azure pipeline
    https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml
how to use user inputs
how to use multi stage pipeline
how to deploy on to AKS - may cover in AKS course
how to use passwords in pipeline
```

## Pipeline Analytics
When you have multiple runs of your pipeline, you will can see reports about the summary of the different runs
For for any pipeline, first you have a summary of all of the runs of the pipeline

So first in Azure Pipelines, you can go to your desired pipeline. Taking an example in the below snapshot

![](https://img-c.udemycdn.com/redactor/raw/2020-09-22_11-23-56-fe03e3e8d81e203ab28768d076b955db.jpg)

Here you will see a summary of all of the runs of that pipeline

![](https://img-c.udemycdn.com/redactor/raw/2020-09-22_11-24-39-f7863ed72563069ab745b8cfb4046736.jpg)

Then you can go to the branches tab. Here you will see a graphical representation of the runs of the pipeline against the various branches

![](https://img-c.udemycdn.com/redactor/raw/2020-09-22_11-26-07-472b3af5a1fd0afe408e175097e27470.jpg)

The red bar indicates that the pipeline failed during that run.

Next when you go to the Analytics tab

![](https://img-c.udemycdn.com/redactor/raw/2020-09-22_11-26-55-f7ac397fc03a62744195e86c628912c6.jpg)

Here you can see reports on the various pipeline runs

![](https://img-c.udemycdn.com/redactor/raw/2020-09-22_11-27-42-ac9654252c6258750d44c25a37b25618.jpg)

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

# Trouble shooting Pipeline
**Errors** under build / release pipeline summary
**Job** will list all the tasks in it and check out the log of a failed task.

# Selenium
- Its a WebUI testing frame work.
- Any of the programming language can be used along with the frame work i.e .net to test the WebUI
- we can test the page contents and different controls on the page without manual intervention

# Azure Pipelines - GitHub
https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/github?view=azure-devops&tabs=yaml

## Github status badge
AZ-400 : 369

# Azure Pipelines - Teams
1. Install `Azure pipelines` app in Teams
2. Subscribe to `Pipeline URL`

**1. Install `Azure pipelines` app in Teams**
Teams -> Apps -> Azure pipelines
![](Pasted%20image%2020220817215519.png)

**2. Subscribe to pipeline URL**
![](Pasted%20image%2020220817215742.png)


# Azure pipeline - caching
https://docs.microsoft.com/en-us/azure/devops/pipelines/release/caching?view=azure-devops
- Reduces build time.
# Azure Pipeline - Terraform
1. Create terraform file for creating a VM and push to repo
2. ![](Pasted%20image%2020220809140939.png)

3. Create a build pipeline and publish the terraform files to be available for release pipeline
```yml
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml
trigger:
- master
 
pool:
  vmImage: 'ubuntu-latest'
 
steps:
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(Pipeline.Workspace)'
    artifact: 'drop'
    publishLocation: 'pipeline'
```

4. Create an empty build pipeline and add tasks as below
Azure devops Portal -> Pipeline -> Release pipeline -> empty job
![](Pasted%20image%2020220809134304.png)
    a. Add previous build job's artifact
    b. Add Job
        ![](Pasted%20image%2020220809134516.png)
    ![](Pasted%20image%2020220809134608.png)
    Add Azure CLI task to create storage account
![](Pasted%20image%2020220809134758.png)
Mention the service connection to connect to storage account.

![](Pasted%20image%2020220809134844.png)
![](Pasted%20image%2020220809134924.png)
inline script
```sh
az storage account create -g newgrp1 -n terraform10001 -l centralus --sku Standard_LRS
az storage container create --name terraform --acount-name terraform10001 
az storage account keys list -g newgrp1 --account-name terraform10001 --query "[0].value" --output tsv > temp.txt
$content = Get-Content temp.txt -First 1
$key = '"{0}"' -f $content
```

C. Add another task - Terraform tool installer
![](Pasted%20image%2020220809135142.png)
Note: Install `terraform` extension if needed.

![](Pasted%20image%2020220809135255.png)
Leave everything defaults

D. Add another task - Terraform
![](Pasted%20image%2020220809135344.png)

![](Pasted%20image%2020220809135443.png)
Display name can be anything here its Terraform: Init for our convenience
 
Configuration directory - browese and select the folder contains TF config files
![](Pasted%20image%2020220809135615.png)
![](Pasted%20image%2020220809135651.png)

![](Pasted%20image%2020220809135922.png)
This is for the remote tf state file
Storage account and Container - Mention the stoarge account name created in the first step. 

![](Pasted%20image%2020220809140207.png)
![](Pasted%20image%2020220809140146.png)

Add another task - Terraform-Plan
![](Pasted%20image%2020220809140431.png)
All other options as above terraform - init

Add Another task - Terraform - validate and apply
![](Pasted%20image%2020220809140650.png)
![](Pasted%20image%2020220809140801.png)
All other options as above terraform - init

Run the release pipleline would create a VM in Azure.

# Deployment strategies
1. Blue green deployment
    ![](Pasted%20image%2020220718122108.png)
2. Canary deployment
    ![](Pasted%20image%2020220718121951.png)
1. Rolling deployment
2. Feature flags

# Build Your Infrastructure
![](Pasted%20image%2020220814174217.png)


# Security in CI/CD Pipeline
![](Pasted%20image%2020220807122948.png)
Security in Development stage
- Using security plugins in IDEs
- Conduct peer reviews
- Adhere to coding standards

## Different types of testing
- Unit testing
- Code coverage
- Code metrics
- Static/Source code analysis
- Functional testing
- Load Testing

### Unit Testing
![](Pasted%20image%2020220807123531.png)
- App would have multiple mdules and each module will be tested by its respective unit test cases.
- Automated frame works to develop unit tests are Nunit and Junit etc..

**Unit Test cases in Azure Pipeline**
1. Develop the unit test cases for your project
2. Add/Push them in to your repo
3. Add the task in pipeline 
    Sample YAML
```yml
    # ASP.NET Core (.NET Framework)
    # Build and test ASP.NET Core projects targeting the full .NET Framework.
    # Add steps that publish symbols, save build artifacts, and more:
    # https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core
 
trigger:
- master
 
pool:
  vmImage: 'windows-latest'
 
variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'
 
steps:
- task: NuGetToolInstaller@1
 
- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'
 
- task: VSBuild@1
  inputs:
    solution: '$(solution)'
    msbuildArgs: '/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:DesktopBuildPackageLocation="$(build.artifactStagingDirectory)\WebApp.zip" /p:DeployIisAppPath="Default Web Site"'
    platform: '$(buildPlatform)'
    configuration: '$(buildConfiguration)'
 
- task: DotNetCoreCLI@2
  inputs:
    command: test
    projects: '**/*Test/*.csproj'
    arguments: '--configuration $(buildConfiguration)'
```
Here the unit test cases are in the `unitTest` folder so in yaml mentioned it as `*Test`.

4. Check the test case results
![](Pasted%20image%2020220807140140.png)

### Code coverage
- ensures all the modules are tested through unit tests
- ensures all the modules are being used in the app, or in other words there is no unnecessary code in the app.
- use coverlet.msbuild extension to test the code coverage for .net projects.
### Code metrics
- ensures the maintanability of the code by measuring its complexity

### Static/Source code analysis
- Based on set of rules the SCA tools check for the security concerns in the code and suggests on improving the code.

> All these testing (Unit, code coverage, code metrics and SCA) can be done in development phase as well as in integration phase.
> 

### Automated functional testing
- Tool that helps in testing system as a whole.
- ex Selenium

### Load testing
- simulated user session to put load on the system and test.

## Adding Unit tests
1. With in the solution/product define unit tests under another project.
2. Add unit test to the pipeline
![](Pasted%20image%2020220817182035.png)
Note: webtest.csproj is the project having unit tests and .csproj is the .net project file extension

## Mend (White source) bolt with Azure devops
WhiteSource bolt for Azure DevOps is a FREE extension, which scans all your projects and detects open source 
components, their license and known vulnerabilities. Not to mention, we also provide fixes. 

- This is an open-source software security and compliance management tool.
- It integrates with the Azure DevOps set of tools , in the build pipelines.
- Check for any sort of security vulnerabilities , licensing issues.
- Let's you know if you are using outdated libraries.

**1. Install White source bolt** extension 
Azure devops -> organization -> settings -> extensions -> "Mend formerly WhiteSource bolt" 

**2. Add Task to your pipeline**
![](Pasted%20image%2020220807134838.png)
Note: it doesnt need any inputs, just remove if its added by default.

**3. Check WhiteSource bolt Report**
![](Pasted%20image%2020220807135056.png)

> Note: White source bolt free version is limited to 5 scans per day per repo.

Sample YAML with white source bolt
```yml
# ASP.NET Core (.NET Framework)
# Build and test ASP.NET Core projects targeting the full .NET Framework.
# Add steps that publish symbols, save build artifacts, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core
 
trigger:
- master
 
pool:
  vmImage: 'windows-latest'
 
variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'
 
steps:
- task: NuGetToolInstaller@1
 
- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'
 
- task: VSBuild@1
  inputs:
    solution: '$(solution)'
    msbuildArgs: '/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:DesktopBuildPackageLocation="$(build.artifactStagingDirectory)\WebApp.zip" /p:DeployIisAppPath="Default Web Site"'
    platform: '$(buildPlatform)'
    configuration: '$(buildConfiguration)'
 
- task: WhiteSource Bolt@20

```

## SonarQube
Code quality and security.

## Integrate with Azure Devops
**1. Install Sonal cloud extension on Azure devops**
Azure devops -> organization -> organization settings -> extension -> sonar cloud

**2. Create service connection to connect to soarcloud from azure devops**
Azure devops -> organization -> service connections 

**3. Add Tasks to pipeline**
    - SonarCloudPrepare
    - SonarCloudAnalyze
    - SonarCloudPublish

Sample YAML
```yml
# ASP.NET Core (.NET Framework)
# Build and test ASP.NET Core projects targeting the full .NET Framework.
# Add steps that publish symbols, save build artifacts, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core
 
trigger:
- master
 
pool:
  vmImage: 'windows-latest'
 
variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'
 
steps:
- task: NuGetToolInstaller@1
 
- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'
 
- task: SonarCloudPrepare@1
  inputs:
    SonarCloud: 'sonar-connection'
    organization: 'app-org'
    scannerMode: 'MSBuild'
    projectKey: 'app-project'
    projectName: 'app-project'
- task: VSBuild@1
  inputs:
    solution: '$(solution)'
    msbuildArgs: '/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:DesktopBuildPackageLocation="$(build.artifactStagingDirectory)\WebApp.zip" /p:DeployIisAppPath="Default Web Site"'
    platform: '$(buildPlatform)'
    configuration: '$(buildConfiguration)'
 
- task: SonarCloudAnalyze@1
- task: SonarCloudPublish@1
  inputs:
    pollingTimeoutSec: '300'
```

**4. Results and issues will be available in Sonar Cloud**

GAPS: Technical debt - vid 80
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
with Azure Repos you can use Git or Team foundation version control system.

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

# Design and Implement IAC
Microsoft has released a new language called Bicep that has the same capabilities as ARM templates.
Bicep just uses a syntax that is easier to use.