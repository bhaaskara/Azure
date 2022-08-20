Access azure devops @ `https://dev.azure.com`

# System / Built in Variables

Build.BuildID
Build.SourceVersion
System.DefaultWorkingDirectory
Build.ArtifactsStagingDirectory

https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml

# Create project in AZDevops
1. Logon to dev.azure.com
2. click on create project
![](Pasted%20image%2020220525203859.png)
Note: Under work item process we can select Basic, Scrum or Agile which creates an empty template.

# Boards
Once you create a project, look at the boards with in that project.

![](Pasted%20image%2020220525204544.png)
**Work items**
**Epic**: is very high level, leader ship team may define a project as an Epic.
**Feature**: Epic/Project contains features
**User story**: contains tasks
**Tasks**

## Integrate Boards with Github
Azure devops -> Project -> project settings -> github connections -> Authorize azure boards to access Github -> add repo
![](Pasted%20image%2020220719160558.png)
and In Github it install Azure boards extension.
it can be seen in Github -> Repo -> settings -> Integration

while committing the code use the keywords "Fixed AB#79" (79 task id) as commit message will make Azure boards to close the task.
![](Pasted%20image%2020220719161206.png)


# Repos
## Create a pull request
To create a pull request, enable branch policy in Repos
![](Pasted%20image%2020220718230734.png)
![](Pasted%20image%2020220718230816.png)
![](Pasted%20image%2020220718230847.png)
![](Pasted%20image%2020220718231033.png)
![](Pasted%20image%2020220718231106.png)
![](Pasted%20image%2020220718231145.png)

![](Pasted%20image%2020220718231221.png)
add another reviewer
![](Pasted%20image%2020220718231321.png)
Now the other user has to approve the pull request
Once its approved by the reviewer
Go to pull request and complete the merge
![](Pasted%20image%2020220718231505.png)
![](Pasted%20image%2020220718231523.png)

# Pipeline
## Create a build pipeline
**Azure DevOps - Build and Push Docker Image to Azure Container Registry**
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/19-Azure-DevOps-with-AKS/19-01-Azure-DevOps-BuildandPush-to-ACR

## Build pipeline - Task to deploy on AKS
![](Pasted%20image%2020220722210150.png)

## Pipeline Task - Install .Net core
Use the task `use .net core`

![](Pasted%20image%2020220817095941.png)


![](Pasted%20image%2020220817102553.png)

![](Pasted%20image%2020220817102643.png)

## Pipeline Task - Publish .Net core build
![](Pasted%20image%2020220817120119.png)

# Release Pipeline - Deploy webapp
GAPS 383,384 - AZ 400 designing and implementing devops certification 2022
