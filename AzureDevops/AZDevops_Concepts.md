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

Bug fixes or feature developments on feature branches will be merged on to master/main branch through pull requests.

## Azure Pipelines
### Create a New pipeline
Azure Devops -> Project -> Pipelines -> Create/New pipeline
![](Pasted%20image%2020220716230328.png)

Select the Repo for your pipeline yaml
![](Pasted%20image%2020220716231424.png)
![](Pasted%20image%2020220716231456.png)
Note: Depending on the code in branch azure pipeline would suggest pipeline types
          i.e .net, java or python and a new pipeline with suggested steps will get populated and you can change it if needed.
          
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

## Ref
**YAML schema reference for Azure Pipelines**
https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines

**Microsoft hosted agents**
https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml