Access azure devops @ `https://dev.azure.com`

# Create project in AZDevops
1. Logon to dev.azure.com
2. click on create project
![](Pasted%20image%2020220525203859.png)
Note: Under work item process we can select Baisc, Scrum or Agile which creates an empty template.

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