# Usefull git commands:
```bash
# General
git checkout <BRANCH_NAME>
git branch -D <BRANCH_NAME> 
git branch --all  
git log --oneline  
git rebase -i <HEAD~5>  
git rebase --abort  
git commit --amend  
git push -f origin <BRANCH_NAME>
git reset --hard <HEAD>  
git restore -s@ -SW -- <FOLDER>

# Delete branch
git push origin --delete <BRANCH_NAME>

# Rename the current branch:
git branch -m <newname>

# Rename a branch while pointed to any branch:
git branch -m <oldname> <newname>

# Correct wrong commit layout
git commit --amend
git push -f origin FACESWP-16235-DIDDiscovery

# Get even with master (Corrects boot failure if your branch is to behind from master)
git fetch && git pull && git merge origin/master

# Squash and rebase commit
git rebase -i origin/master
git rebase --abort
git add .
git rebase --continue
git push -f

# Apply commit to your branch
git cherry-pick <COMMIT_ID>

# Resolving a git conflict with binary files
git checkout --theirs -- path/to/conflicted-file.txt
git checkout --ours -- path/to/conflicted-file.txt

# Pull all repos at once
find . -mindepth 1 -maxdepth 1 -type d -print -exec git -C {} pull \;
```
> Git info: https://confluence.dt.renault.com/display/FACEasy/Useful+Git+Commands

### Docker examples:
> Docker on WSL
```bash
sudo service docker start 

# scp
scp <FILE> <USER>@<IP>:<PATH>
```

### Commit message examples:
```bash
<type>[(<scope>)]: <subject>

<problem statement>

<solution>

[BREAKING CHANGE: <impact>]

Closes <ID>
```

> Commit info: https://www.conventionalcommits.org/en/v1.0.0/

# Usefull SSH commands:

```bash
sudo ssh-keygen -A
sudo service ssh status
sudo service ssh start
ssh renault@tldlab046
ssh renault@<TABLE_IP>
```

# Usefull debug commands:
```bash
all the logs
journalctl
journalctl -xe

systemctl status <SERVICE>

Stop a container
systemctl stop <SERVICE>

Launch a container
systemctl start <SERVICE>

Stop/launch a container
systemctl restart <SERVICE>

```

# Usefull WSL commands
```bash
wsl -l -v
wsl --status
wsl --version
wsl --shutdown
wsl --mount <DiskPath>
wsl --unmount <DiskPath>
wsl --export <IMAGE_NAME> <BKP_NAME>.tar
wsl --import <IMAGE_NAME> <BKP_NAME>.tar
wsl.exe -l -v
wsl.exe --set-version (distro name) 2
wsl.exe --set-default-version 2
wsl --set-default ubuntu

# Backup WSL
wsl --list
wsl --shutdown
wsl --export <Image Name> <Export location file name.tar>
```
