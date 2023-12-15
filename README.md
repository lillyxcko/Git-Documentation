# Disclaimer
This comprehensive guide, originally created for internal use at TK Graphics, provides step-by-step instructions for setting up Git in both local and server environments. Presented in GitLab-flavored markdown, this guide was created to address the lack of an easily accessible, all-encompassing resource on this topic online.

<br><br>
<img src="https://tkgraphics.ca/wp-content/uploads/2021/03/tk-graphics-logo.png" width="233" height="57">

# Hello! 👋
>This is your guide to setting up Git's Version Control system. It includes both local environment and server set-up. 

> 🌱 Please make sure you have the [latest version of git installed](https://git-scm.com/downloads). You may configure all based on Git's recomended default selections.

>Authored: Lilly Ko 2023-12-03

<br>

#### Table of Contents:
- [Adding SSH Key to GitLab](#adding-ssh-key-to-gitlab-to-allow-pushpull) 
- [GitLab on your Local Environment](#gitlab-on-your-local-env)
- [Create Git Command Shortcut](#configure-git-command-shortcut)
- [Setting Up SSH Key Authentication on Server](#setting-up-ssh-key-authentication-on-server)
- [Git IDE Configuration](#git-ide-configuration)
- [Configure Git Repo on Server](#configuring-git-repo-on-server)
- [Troubleshooting Common Errors](#troubleshooting-common-errors)

<br><br>

# Adding SSH Key to GitLab (to allow push/pull)
>The following is to be done in your local environment. 

### 1. Generate SSH Key Pair:

If you don't have an SSH key pair, you need to generate one:

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

This command will prompt you to specify a file to save the key. Press [Enter] to set as default. You don't need to set up a password. Press [Enter] twice to proceed without password. 


### 2. Add SSH Key to GitLab:
Copy the SSH public key to your clipboard:

`cat ~/.ssh/id_rsa.pub`

Log in to your GitLab account on your web browser.

Click on your avatar and go to Edit Profile.

In the left sidebar, click on SSH Keys.

Paste your SSH public key (copied in step 3) into the "Key" field. [*keep your SSH key for [Setting up SSH Key Auth process](#setting-up-ssh-key-authentication-on-server)]

Optionally, give your SSH key a recognizable title in the "Title" field. You can accept the default title suggestion which will be your email.

Click Add Key.

### 3. Test the SSH Connection:
Test the SSH connection to make sure it's working correctly:

`ssh -T git@gitlab.com`

GitLab should respond with a welcome message.


<br><br>


# GitLab on your Local Env.
>Pre-requisite: [Install Git](https://git-scm.com/downloads)
### 1. Initialize Git in a directory:
Create a directory to hold the repository's code. 

Open Git Bash on your local computer and navigate to the directory.

Run the following command in that dir to initialize a Git repository inside:

`git init -b main`

This also sets the default branch name to "main". 

`git remote add origin git@gitlab.com:tkgraphics/repo-name.git`

Replace repo-name with the name of the remote repository set-up in GitLab. TK's repo names should all be in lowercase.

if you have accidentaly create a origin remote already, you can remove origin remote by:

`git remote rm origin`

### 2. Clone the GitLab Repository:
In your local directory where you want to clone the repository,

use the following command to clone from the GitLab repository:

`git clone https://gitlab.com/tkgraphics/repo-name.git`

Replace repo-name with the name of the GitLab repository.

It will prompt you to log into your GitLab. Login to the GitLab account which has access to the TK repos. 

⚠ git clone may make another dir within your dir (e.g. TK15 > tk15). It is suggested to only have one level (e.g. TK15) so rearrange the files accordingly.

#### If there is no GitLab repo, or you want to clone directly from the server into your local:

>`git clone tk_admin@server-ip-address:/var/git/web.git`

>Now the code from the server should be in your local directory.

>To test if your local can be pushed to the repo, make changes in your local code, then:

>`git add.`

>`git commit -m "commit-message"`

>`git push origin main`

>This pushes the code from your local directory to the remote repository.

>Confirm it on GitLab. If you have competed this subset of tasks, you can skip #3, 4, 5, 6, 7

### 3. Move into the Cloned Repository:
Navigate into the directory of the cloned repository:

`cd your-repository`

### 4. Make Changes to the Project:
Make the necessary changes to the files within the repository using your preferred text editor or IDE.

### 5. Apply the local changes to the repository - Pull, Stage and Commit Changes:
Make sure your local copy is up-to-date with the repo.

`git pull`

If you do this after `git add` and your local is not up-to-date it will prompt you to merge.

Stage the changes you want to commit using the following commands:

`git add .`

This stages all changes. If you want to stage specific files, replace . with the file names.

Optional: Check that your intended files have been added
`git status`
This will tell you what has been modified and staged for commit.


Commit the staged changes:


`git commit -m "Your commit message"`

Replace "Your commit message" with a brief and descriptive message about the changes you made.

Read the message prompt carefully. If the operation is not successful, follow its suggestions to configure your account accordingly. 

### 6. Push Changes to GitLab:
Push your committed changes to the GitLab repository:

`git push origin main`
This command assumes you are pushing changes to the repository's main branch. 

If you are working on a different branch, replace master with the name of your branch. 

(See #7 on how to push directly to the live server)


### 7. Verify Changes on GitLab:
Visit your GitLab repository in a web browser and navigate to the repository's "Commits" section. Verify that your recent commit is listed.


### 8. Push Changes to Live Server:
#####      a. Check Git Remotes:
In your local repository workspace, use this command to check what remotes your Git has:
`git remote -v`

A remote is like a "path" you want your code files to take 
     
At this point there should be by default an "origin" remote which points to the Git Repository. 

If there are no remotes which have a url pointing to the live server's IP Address, you can add one as outlined in step b.
#####      b. Add Git Server Remote:
`git remote add live ssh://tk_admin@server-ip-address/var/git/web.git`

The name of your new remote will be called "live". It is suggested to keep it as "live". You may replace "live" with a custom name if needed.

Note: the path specified above is the default path to Git repos that are set up in TKG's servers. 

You will get a message saying "Are you sure you want to continue connecting...". Type "yes" in the terminal.

#####      c. Push to Server:
`git push live`

If you had chosen a different remote name, replace it over "live".

Note: you should finish setting up the [SSH Key Auth on the Server](#setting-up-ssh-key-authentication-on-server) before pushing to it.

If it gives you a branch tracking error, run this line in the terminal before trying to push again:

`git branch --set-upstream-to=origin/main main`

Now your code should be pushed onto the server of IP address you specified in the remote set-up in #b. Check if your push has been succesful by checking the live server. 

<br><br>

# Configure Git Command Shortcut
>Condenses the 5 Git commands to get code on the live server into 1 command.

1. Make sure you are in your local home directory:
`cd ~` 

2. Open the git config file
`git config --global --edit`
  
3. With the file opened in bash, press the letter [I] to enter INSERT mode. Keep ay exiting text in the file while pasting the following into config file:
```
[alias]
    export = "!f() { git pull && git add . && git commit -m \"$*\" && git push origin && git push live; }; f"
```

⚠ You need to make sure the git push remote names correspond to the remote names you have set-up. you may need to replace “origin” in git push origin with your GitLab repo remote name, and “live” in git push live with the remote name  you have set-up for the sever.

You can check your remote names with the following command: `git remote -v`

If the remotes do not appear, create remotes with these commands:

GitLab Repo - `git remote add origin git@gitlab.com:tkgraphics/your-repo.git`

Server - `git remote add live ssh://tk_admin@server-ip-address/var/git/web.git`

4. Save the changes in git config file. Here are instructions on how to save files on Nano editor:
```
    - Press Ctrl + O (that's the letter O, not zero).

    - Press Enter. 

    - To exit Nano, press Ctrl + X.
```

5.  Navigate to your local git repo dir and test the shortcut in your IDE or GitBash terminal:

`git export "this is a commit message"`

If it gives you a branch tracking error, run this line in the terminal:

`git branch --set-upstream-to=origin/main main`

Try the shortcut again.


<br><br>

# Setting up SSH Key Authentication on Server[*]
>Every user who pushes code to the server should have SSH Auth set up for their devices.


### 1. Log in to the Server:

Log in to your server using a password:

`ssh tk_admin@server-ip-address`

### 2. Add Public Key to authorized_keys:

([Remember the SSH key generated earlier.](#3-add-ssh-key-to-gitlab) It should match the SSH key you added to your Git profile.)

You can double check your key on your local computer by:
`cat ~/.ssh/id_rsa.pub`

Copy the key that appears (staring in "ssh-").

If no key appears you need to create one:

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

`cat ~/.ssh/id_rsa.pub`


Create or edit the ~/.ssh/authorized_keys file:

`nano ~/.ssh/authorized_keys`


Paste the copied public key into this file. Here are instructions on how to save files on Nano editor: 
```
    - Press Ctrl + O (that's the letter O, not zero).

    - Press Enter. 

    - To exit Nano, press Ctrl + X.
```


### 3. Adjust Permissions:

Make sure the permissions are set correctly:

`chmod 700 ~/.ssh`

`chmod 600 ~/.ssh/authorized_keys`

### 4. Configure SSH Agent:

`eval "$(ssh-agent -s)"`

### 5. Test if Configured: 

`logout`

`ssh tk_admin@server-ip-address`

If it does not prompt you to enter a password SSH has been configured successfully. 

<br><br>

# Git IDE Configuration

>IDE: <a href="url"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/PhpStorm_Icon.svg/2048px-PhpStorm_Icon.svg.png" align="left" height="25" width="25" ></a> PHPStorm 

>You need PHPStorm Version 2023.2 or greater to integrate GitLab

### 1. Open your project folder in the IDE.

   Navigate: “Settings” (Ctrl+Alt+S) > “Version Control” > “Git”

   Make sure the Path to Git executable is set correctly. It should point to where Git is installed on your computer. 

   Have the following fields checked: 
   * [x] Auto-update if push of the current branch was rejected
   * [x] Use credential helper : Apply

### 2. Create a Personal Access Token

   Go to your GitLab web browser. 

   Navigate to your GitLab account that has access to the TK repositories. 
   
   Click on your avatar and go to “Profile Settings” > “Access Tokens”

   Configure scopes accordingly:
   * [x] api
   * [x] read_user
   * [x] read_repository
   * [x] write_repository

    Create and Copy your personal access token.

### 3. Install GitLab

   Go to “Settings” > “Plugins” > “Marketplace”
   
   Search for the GitLab plugin and install it. 

### 4. Add GitLab account to IDE

   Go to “Settings” > “Version Control” > “GitLab” 
   
   Add account by pasting the personal access token you copied in step 5. 
   
   Click Apply/ OK

Now you can git pull/add/commit/push from the IDE terminal.

<br><br>

# Configuring Git Repo on Server
>[FOR ADMIN ONLY] This only needs to be done once when the server is initalized. Skip this if you are only setting up Git on your local environment. 

### 1. Log in to the Server: 
`ssh tk_admin@server-ip-address`

### 2. Create a Bare Git Repository in the Server:
```
cd /var

sudo mkdir git

sudo chown -R tk_admin:www-data /var/git

sudo chmod -R 775 /var/git

cd git

git init --bare --shared web.git && cd $_
```
Once finsihed, the path to the bare Git repository should be: /var/git/web.git

### 3. Post-Receive Hook:
```
cd hooks

sudo nano post-receive
```
This enters the hooks directory and creates a post-receive hook. 

inside the file, paste: 
```
#!/bin/sh

git --work-tree=/var/www/html/api --git-dir=/var/git/web.git checkout -f main
```
The hook will checkout the code to the main branch, inside the directory being read by apache2 server (path to directory is specified by: work-tree).

⚠ It is very important that the paths here are set correctly. If updates are not being reflected in the server it is likely the issue lies in the paths. 

set permissions:

```
sudo chmod -R 775 /var/git/web.git

sudo chown -R tk_admin:www-data /var/git/web.git

cd ..

chmod +x hooks/post-receive
```

### 4. Testing:
After [setting up the Git remote](#8-push-changes-to-live-server) to this server, you can push code to it by: 

`git push live`

If you chose a different name for your server remote, place it over "live".
 
<br><br>
# Troubleshooting Common Errors

```
From gitlab.com:tkgraphics/tk103
   32dae76a..2db6f69b  main       -> origin/main
error: Your local changes to the following files would be overwritten by merge:
Updating 32dae76a..2db6f69b
```

Why: This error message from Git indicates that you have changes in your local repository that haven't been committed, and Git is preventing you from pulling changes from the remote repository (in this case, from the branch main on GitLab) because it would overwrite your local changes.

Resolution: 

`git stash`

`git pull`

`git stash pop`

Now you can try to "[git export](#configure-git-command-shortcut)" again. 
