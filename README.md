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



