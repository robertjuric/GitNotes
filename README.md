# GitNotes

My personal notes for common Git CLI tasks. 

## Getting Started with GitHub
---

### Setting Up SSH
1. Create new SSH key on dev machine and add it to ssh-agent  
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux  
	```
	$ ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"
	```

	* Save it to the default location, optionally enter a passphrase for the key.
	* Then add the key to the ssh-agent. Use the location from the previous step.

	```
	$ ssh-add ~/.ssh/id_rsa
	```

2. Add SSH key to GitHub Profile  
https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-linux  
	* Install xclip to copy the file to the clipboard. Use the location from the previous steps.

	```
	$ sudo apt install xclip
	$ xclip -sel clip < ~/.ssh/id_rsa.pub
	```

	* Then login the GitHub.com website and under your settings click 'SSH and GPG keys' and then 'New SSH key' and paste the key.

## The Basics
---

### Creating
Creating a new repository from code on a machine to a remote cloud repo.
In the directory containing your project:
```
	git init
```
Add the files with `git add`.  
Commit to the local repository with `git commit -m "Message"`.  
Create the repository on the cloud.  
Add the remote repository and link/push to the remote repo:
```
	git remote add origin git@github.com:username/new_repo
	git push -u origin master
```
The `-u` flag specifies an `upstream` branch and is used as a tracking reference. The `origin` statement defines it as a remote repository and `master` is the branch name. Once the upstream branch has been set you can use argumentless commands.  
From here you are able to commit locally and push to the remote repo as normal.

### Cloning
Used to copy from repository to repository. Commonly used to clone from a remote cloud repo such as GitHub or GitLab to a local repository.
```
	git clone ssh://user@example.com/project.git /directory/path
```	
* If clone was done via HTTPS, need to set-url. If clone was done via SSH, skip this step.

```
	git remote set-url origin git@github.com:username/your-repository.git
```
### Committing
Used to commit changes to the local repository.

You have to add the changes to the staging area first:
```
	git add filename.ext
```	
You can then commit the changes:
```
	git commit
```	
If you want to commit a snapshot of all changes in the working directory (everything that has been previouly added):
```
	git commit -a
```	
If you want to commit with a comment (you should):
```
	git commit -m "Change summary"
```	
If you want to combine the snapshot with a comment:
```
	git commit -am "Change summary"
```	
### Pushing
Used to push commits to a remote branch.
```
	git push <remote> <branch>
```	
### Pulling
Used to merge changes from remote repo to the local repo.
```
	git pull <remote>
```
## Advanced
---

### Basic rebase
If remote repo is a commit ahead of the local repo --rebase will allow you to pull the changes from the remote and stack your changes on top. Commonly used for resolving merge/push conflicts:
```
	git pull --rebase origin
```	
### Branches
If you want to create a new feature you can create a new branch of a repository commit to it separately from the master branch. You can then use a pull request to merge the branch into master.

The first thing to do is create (`-b` flag) and checkout the new branch from master:
```
	git checkout -b newfeature
```	
To make changes in the new branch, you can use `git status` to verify the branch that you're in and then commit and push changes same as before.

When you're ready, you can push your changes to the remote repo. Use the `-u` flag to add it as a remote tracking branch.
```
	git push -u origin newfeature
```	
Since the branch was configured as a tracking branch you can use the standard `git push` moving forward. From there you can go into the repo management tool (GitHub, GitLab, etc.) and issue a Pull Request to merge the 'newfeature' branch into 'master'.

##### Merging a Branch
To merge a branch into master, you'll have to use `checkout` to move back to master branch, `pull` to grab the latest changes, `pull` the branch in, and then `push` the changes back to the remote repo.
```
	git checkout master
	git pull
	git pull origin newfeature
	git push
```	
