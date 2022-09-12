# RemoteResearchTools

## Setting up Github SSH keys

1. On your terminal, create a custom ssh key to use with github ```ssh-keygen -t rsa -b 2048 -C "https://kunalmgupta.github.io"```. Use your own email ID or some other string. 
2. Save key to location (custom for github)  ```/Users/kunal/.ssh/github```. This is going to create a private key ```~/.ssh/github``` and public key ```~/.ssh/github.pub```.
3. View and then copy the public key using ```cat  ~/.ssh/github.pub```. And put this in your github account https://github.com/settings/keys.
4. Since we made a custom ssh key, we would have to append this to ```~/.ssh/config/```. If it is not currently there then create a new one! ```vim ~/.ssh/config/```
5. Add the following to the config file. Replace ```/Users/kunal/.ssh/github ``` with your own ssh key path. 
    ```Host github.com 
      HostName github.com 
      User git 
      Port 22 
      PreferredAuthentications publickey 
      IdentityFile /Users/kunal/.ssh/github 
      IdentitiesOnly yes```
  
  6. Check if authentication is working ```ssh -T git@github.com```. If it says something like "Hi KunalMGupta! You've successfully authenticated, but GitHub does not provide shell access." Then Congratulations! you are all set. 

## When you have a new git repo
1. Create a new repository on your git account. Then git clone it to your machine. 
3. Add username to git ```git config --global user.name "KunalMGupta"```
4. In your folder, add the remote. For instance, for this repo you will do the following. 
     ```cd RemoteResearchTools``` and ```git remote set-url origin git@github.com:KunalMGupta/RemoteResearchTools.git``` 
5. Now you can do your usual add/commit/push to remote. Enjoy!
  
## Useful Github commands
1. To see the log of all commits: ```git log```
2. To see the files that are currently being tracked ```git ls-tree -r master --name-only```
3. To push files ```git push origin main```
4. Set email ```git config --global user.email k5gupta@eng.ucsd.edu```
5. Set username ```git config --global user.name KunalMGupta```


## Having a GUI access to remote server. 
I have found that a combinatino of nomachine and teamviewer works out quite well for this. The catch is that Teamviewer generally gives much better bandwidth compared to nomachine, however I couldn't get to start from ssh terminal itself, luckily this issue is not with nomachine. So, the trick is to first use nomachine to launch teamviewer (only if it not running automatically). Once the teamviewer is active, we can simply use that. 

1. Install nomachine on both your machine (client) and linux server (host). For downloading instructions look here for <a href="https://www.nomachine.com/download/download&id=7"> Mac OS</a> and <a href="https://knowledgebase.nomachine.com/AR01L00775">Linux </a>. Also, I recommend first downloading both of these to your local machine and then scp the linux installer to the server. 
2. No machine will automatically get activated after the installation. Connect to server via your username and password (using nomachine). 
3. Once in, you can simply connect using teamviewer and close nomachine later on. 

## Mounting Remote dirs on local machine (Mac OS)
Sometimes it is handy to be able to use finder to quickly parse through the directories on your remote machine. For this we use <a href="https://cyberduck.io"> Cyberduck </a> applications that relies on SFTP and is super easy to set up and is free (but I encourage donating if you like it!). 

## Backing up data to GDrive.(from linux server)
On several occasions there is a need to upload data from the servers to a place where it can be shared more easily with others. I found that using <a href="https://rclone.org/drive/"> rclone </a> to be a great tool to achieve this. To back up some file ```backup.zip``` to your GDrive, do the following. 

1. First install rclone on your linux server ```apt install rclone```
2. Then config your Google account with rclone using ```rclone config```
3. Give some name say ```drive``` to refer to the remote location
4. Since we are using GDrive, type ```drive``` next
5. Leave blank for Client Id and Secret
6. Since we don't have access to a monitor on the remote server, type ```n``` here to get an external signup link
7. Sign in using the generated link on your local machine and copy paste the generated key under ```Enter verification code```
8. Type ```y``` to verify if the token is correct. and then hit Quit ```q```

The destination is now set under rclone config. To list the contents of your GDrive you can use ```rclone lsd <remote name>:```. Now to do fast copy from your local server to GDrive, use the following command. 

```rclone copy backup.zip drive:<target directory in GDrive>/ -v --drive-chunk-size=512M ```

Here, the ```-v``` flag will tell you things like progress and ETA and the ```--drive-chunk``` is crucial to make this run fast, however, in my experience any value above 128 doesn't help much since the uploading is happening over commodity internet, which isn't that great, but you can always use multiple threads to speed things up. I use k8s so I end up using multiple jobs to run the upload which makes this process fast!

## Setting VS Code with Docker and remote SSH. 

https://medium.com/@minkesh.asati/development-with-the-docker-on-the-remote-server-via-ssh-from-vs-code-ef9e0f2fcbe6

