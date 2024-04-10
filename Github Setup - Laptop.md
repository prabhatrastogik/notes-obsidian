## GIT SSH Key Generation
Create ssh-keys and add to Github

```zsh
cd ~/.ssh 
ssh-keygen -t rsa -C "pr@gmail.com"  # Generate ssh key 
eval "$(ssh-agent -s)"  # Add ssh key to daemon "ssh-agent" 
pbcopy < ~/.ssh/id_rsa.pub # Copy the public key to clipboard # Login to github and add the SSH key on the website.
```

## Multi Account Config

Create separate files to target separate private keys ; Build a common file to Include according to location

### Common File
```
# ~/.gitignore
[includeIf "gitdir:~/Novo/"]
	path = ~/Novo/.gitconfig

[includeIf "gitdir:~/Code/"]
	path = ~/Code/.gitconfig
```

### Work File
```
# ~/Novo/.gitignore
[user]
	email = prabhat@novo.co
	name = Prabhat Rastogi

[github]
	user = "prabhat-banknovo"
 
[core]
	sshCommand = "ssh -i ~/.ssh/id_rsa_novo"
```

### Personal File
```
# ~/Code/.gitignore
[user]
	email = prabhatrastogik@gmail.com
	name = Prabhat Rastogi

[github]
	user = "prabhatrastogik"
 
[core]
	sshCommand = "ssh -i ~/.ssh/id_rsa"
```