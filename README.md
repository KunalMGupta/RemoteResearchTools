# RemoteResearchTools

## Setting up Github SSH keys

1. On your terminal, create a custom ssh key to use with github ```ssh-keygen -t rsa -b 2048 -C "https://kunalmgupta.github.io"```. Use your own email ID or some other string. 
2. Save key to location (custom for github)  ```/Users/kunal/.ssh/github```. This is going to create a private key ```~/.ssh/github``` and public key ```~/.ssh/github.pub```.
3. View and then copy the public key using ```cat  ~/.ssh/github.pub```. And put this in your github account https://github.com/settings/keys.
4. Since we made a custom ssh key, we would have to append this to ```~/.ssh/config/```. If it is not currently there then create a new one! ```vim ~/.ssh/config/```
5. Add the following to the config file. 
    ```Host github.com \
      HostName github.com \
      User git \
      Port 22 \
      PreferredAuthentications publickey \
      IdentityFile /Users/kunal/.ssh/github \
      IdentitiesOnly yes```
  
  6. 
  
