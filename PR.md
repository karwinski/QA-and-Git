                                                                                        QA team/sept-2017 
## CREATING A PULL REQUEST
------
- This tutorial is in case you do not have write acces to a project, if you have, skip the fork part. 
------

First you need to have installed git, if not, follow this [guide](https://git-scm.com/download).


### Fork the project:

Make a fork is get a remote personal copy of a project. 

Go to main page of the repo you want to colaborate with, and click the Fork button in the top-right corner. 

![](https://github-images.s3.amazonaws.com/help/bootcamp/Bootcamp-Fork.png)

### Clone the fork of the project locally:

Make a clone is create a local copy into your local machine(this would be origin). 

open a terminal:

```
git clone git@github.com/user/project.git

cd project
```

### To update changes in original repo add original remote repository as upstream:

`git remote add upstream git@github.com:user/project.git`

### Fetch your upstream develop branch:

`git fetch upstream develop`

### Create a branch for your task from upstream/develop and switch to it:

`git checkout -b <my_branch> upstream/develop`

#### Make your changes in your new branch, taking into account the following recommendations:
- Name your branch with the Jira task ID: JIRA-*/branch_name
- Commit pattern: [OPERATION]: comment
- Try to use useful names in your commits and branches. 
- Be careful with your commit: Not 1 thousand lines nor 1.
- Always in english.
- Don’t repeat the same message for different commits.

### Make changes to improve the code:

Before make any change, be sure you are in the branch you created. 

`....making changes to the code...`

 Make commit:

``` 
git add <file>
git commit -m "message"
```

### Push the branch into your origin (GitHub fork):

`git push origin my_branch` 
	
To confirm your changes are available in the github fork, go to the `github.com/your_user/project` in the branch `my_branch`. 

Here, you should see the changes you made before and that mean your remote copy is updated. 

### Open a Pull Request on GitHub:

Go to the `github.com/your_user/project` in the branch `my_branch` and click the button "New Pull Request".
![]()

Then, you will see a window where you can give a title and description to the Pull Request. This is important because let to the original owner of the project determine what you were trying to do, whether your proposed changes are correct, and whether accepting the changes would improve the original project.

And hit the `Create  pull request` button on this screen.
![](https://help.github.com/assets/images/help/pull_requests/send-pull-request.png)


- Two team members (QA member and another one) must approve your changes. They will mark with +1 comment. Only then, Pull Request can be merged into upstream/develop branch. Please, use “Squash and Merge” option.


- If your PR is rejected, don’t create another branch to fix it. Make your new commits into the same branch and push it again into the same repository and branch (PR will be automatically updated). 
 


### To see useful documentation about PR:

[tutorial of AeroPython project](https://github.com/AeroPython/PyFME/wiki/Tutorial-paso-a-paso-del-flujo-de-trabajo/_edit)

[Chapter 6 of the ProGit second edition](https://progit2.s3.amazonaws.com/en/2016-03-22-f3531/progit-en.1084.pdf)

[Carlos García Git Talk]()

