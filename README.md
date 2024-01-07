# Project Boards
- **Project board** - [SpectraFX - Devexperts JIRA](https://jira.in.devexperts.com/secure/Dashboard.jspa?selectPageId=26205)
- **Project’s FE backlog** - [Spectra FE - Agile Board - Devexperts JIRA](https://jira.in.devexperts.com/secure/RapidBoard.jspa?rapidView=842&view=planning&issueLimit=100)
- **Scrum board (to keep track of sprint’s work flow)** - [SpectrAxe Scrum Board - Agile Board - Devexperts JIRA](https://jira.in.devexperts.com/secure/RapidBoard.jspa?rapidView=842&view=planning&issueLimit=100)

# Initial Working Environment Setup
- Install nvm – Node Version Manager for switching between node’s versions.
- Install node ~16. We use Node 16 on a project.
- Install yarn for scripts execution.

# Git Installation and Config
1. Install Git depending on OS you are using. Follow guide from Atlassian. [Install Git | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/install-git)
2. Create SSH key on your computer:
```
ssh-keygen -t rsa -b 4096 -C your_email@devexperts.com
```
> Note: If you won’t specify a new path, the keys will be created at your default directory. Typically in users/your_user_name/.ssh

3. Copy your id_rsa.pub.
4. Open BitBucket stash -> click on profile icon -> manage account -> on left side bar click on SSH keys -> add key.
> Note: id_rsa.pub typically looks like: ssh-rsa <key_body>email@devexperts.com

# Fixing Longpath on Windows
#### If you work on Windows OS you need to extend the length of the file path before pulling repository’s files. By default it is set up for 260 symbols. To change this behavior, follow next steps:
1. Open the Run dialog box using Windows + R.
2. Type the following in the box and press Enter: regedit.
3. Select Yes in the User Account Control prompt.
4. Navigate to the following path in Registry Editor: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem
5. Open the LongPathsEnabled entry located on the right.
6. Enter 1 in the Value data field and select OK.
7. Close Registry Editor and reboot your PC.

#### After reboot execute next line in the console:

```
git config --global core.longpaths true
```

# Setup npm
#### It is needed to be logged in nexus before packages installation.
```
$ npm login --registry=https://nexus.in.devexperts.com/repository/npm-dx/
```
```
$ npm login --registry=https://nexus.in.devexperts.com/repository/npm-dx-private/
```
> For login and password, you need to submit your username and passwords from devexperts.

# Initial project setup
#### Open stash with a project [Browse RIA Development / dxtf - Devexperts Stas](https://stash.in.devexperts.com/projects/RIA/repos/dxtf/browse)h
1.	`git clone <project_url>` and open it in your IDE.
2.	`git checkout release/276` (branch for Spectraxe project).
3.	`cd dxtrade5-fe-resources` (directory with frontend resources).
4.	`yarn install` (install all required dependencies).
5.	`cd ../proxy` -> `yarn install` (install proxy packages for using proxy).
6.	`cd ../ dxtrade5-fe-resources` (return to dxtrade5-fe-resources for implementing further commands from this directory).
7.	Run commands to check everything works correctly (only for first launch recommended):
- `yarn build` (makes bundle with the latest changes)
- `yarn storybook:local`  (local start storybook)
- `yarn test`  (frontend tests execution)
- `yarn proxy:dev` (Project’s proxy launch. Can login to it using http://localhost:3333/)
> Note : For test credentials, please ask your linear manager.

# FULL BUILD of DXTF
#### In advance is needed to install Java 11 and Apache-maven 3.8.6.
Full guide for build is described [here](https://confluence.in.devexperts.com/display/dxTrade5/New+Developer+-+Setting+up+the+environment)
#### After successful build:
•	`cd dxtrade5-vendors/dxtrade5-spectrafx`
•	run `./run.bat dev` or `./run.sh dev` depends of OS 
#### Then try to open http://localhost:8080/dxtrade5/?vendor=spectrafx&lang=en
#### If everything is ok, you’ll see the login form.

# Development process
### Eslint config.
Eslint is a static code analysis tool for identifying problematic patterns found in JavaScript/Typescript code.
To config eslint on save action:
-	Install Eslint extension for VS code.
-	In Visual Code Studio -> File -> Preferences -> Settings -> Code Actions On Save
-	Edit settings.json with:
```
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
}
```

-	reload VS code.
> Note: Eslint on save starts working when it is preinstalled locally or globally. Eslint extension is searching for the nearest .eslintrc config file to apply all specified rules.

### Create a new branch for ticket.
#### Steps:
- Open dedicated ticket on Jira. 
- Find on the right panel development tab. Click “create branch”.

#### There are several fields in the ticket creation tab:
 
- **Branch type**: branch type should reflect the type of your ticket.

- **Branch from**: release/276.

- **Branch name**: due to long names generated from JIRA-Bitbucket plugin we usually shorten it to 20-30 symbols, left jira key and most meaningful text. It's made to handle branches in console - cause it's hard to handle huge lines without able to copy them.


### Commits
In the commit’s description should be reflected changes which were implemented to project. Shouldn’t be specified ticket name and project, this prefix will be added by internal script.
If used CLI, commit should be like this:
```
git commit -m”changes description”
```

### Pull Request.
When the ticket is completed, for further merging is needed to create a pull request.
#### Steps to create PR:
- In Jira, find the ticket for which you implemented changes.
- Click on “branch” in the development tab on right.
- Fill the fields:

**Title** is autocompleted field.

**Description**:
#### In description usually we specify:
- Goal of ticket.
- links to Story/stories.
- Comments if needed.
#### BitBucket stash supports markdown syntax, we use it in description and comments for PRs.

### Reviewers
For PR review we usually add the ticket’s reporter, our team and colleagues who are affected by these changes.

### Comments to PR.
Reviewers will leave comments on PR. It could be some clarifications or tasks marked with checkboxes. We have unwritten always reply for tasks - with resolutions like “fixed”, “will fix on another PR”, “not clear – let discuss” etc. To left reviewers possibility to take actions if needed.

### Commits gluing.
If there are more than one commits during development is needed to glue it in one for clear commits history and for easy revert changes if needed. 
Usually we do it before merging, when the last approvals are remained.
To glue several commits:
- Firstly, make sure you are on your branch.
- `git branch`
- `git checkout release/276` (changing our branch to 276)
- `git pull` (pulling the latest changes)
- `git switch –` (return to previous branch)
- `git merge release/276` (merge 276 branch into our branch)
- `git reset --soft release/276`  ( undoes all the changes made between release/276 and our branch, and saves all the changes in the index. To make all changes to be ready to be committed again)
- `git status`
- `git commit -m’Message for a new general commit’`
- `git push -f`

### Stand-ups 
On stand-ups we are talking about our main focuses/goals(typically it’s tickets we are working on, PR’s verifications etc.) for this day or for week and about any blocks which lead to delay for work implementation.

### Groomings 
At groomings we review tickets in the backlog, discuss what needs to be done, add clarifications and comments if needed, and evaluate the complexity of the ticket and the labor costs. 

### Time Tracking
The tracked time is a basis for the invoice sent to the client. This means that we need to be as transparent here as it is possible to avoid the clarifying questions.
We track time working on dedicated tickets and also when taking part in project’s discussions, standups, groomings etc.
More information about time tracking - [SpectrAxe - Confluence (devexperts.com)](https://confluence.in.devexperts.com/display/SFX/Process)







