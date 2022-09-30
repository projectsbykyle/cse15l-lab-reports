# Lab Report - Remote Access and the Filesystem 

For this class (and many others), we need to log into a class account on a remote server to do our work. We can access remote files and run programs on servers doing this. 

This tutorial provides a basic walkthrough on how to set up a remote access system that we can use often. 

## Installing VSCode
We first need to download [Visual Studio Code](https://code.visualstudio.com/).

When we open it after installing, we should have a screen like this. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/VSCodeScreenshot.png)

## Remotely Connecting
Now that we have VSCode installed, we can begin remotely connecting to the server for the first time. 

If you are on Windows, you need to first install the tools [here](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui). Remember to install the **client**, **not** the server.

Now, open the terminal in VSCode (**CMD** or **CTRL** +'. Alternatively, you can click **Terminal**, then on **New Terminal** from the dropdown menu). 

Then, we can enter the following command. Replace **username** with your classroom username and **server** with your classroom server. 
```
ssh username@server 

Example: myaccount@ieng6.ucsd.edu 
```
If this is your first time connecting to the remote server, then you may receive this prompt: 
```
⤇ ssh username@server
The authenticity of host can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
Simply enter yes to proceed. 

After, we should then see the following screen. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/SSHLogin.png)

## Trying Some Commands
We can now try using terminal commands on the remote server and on our local computer. 

Here are some that you can try. 

```
cd ~
cd
ls -lat
ls -a
ls <directory name>
cp <directory name>
cat <directory name>
```
Here are some results of the commands listed above. 
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/TryingDifferentCommands.png)

To log out of the remote server, we can use the **exit** command. 


## Moving Files With SCP
Now that we have the basic tools set up, we can begin learning how we can use remote servers. 

One of the most helpful commands that we can use is **scp**, which stands for **secure copy protocol**. 

Its basic format is as follows: 
```
scp <local user's file directory> user@server:<destination directory>

Example: scp ~/HW1.txt studentA@school.edu:~/
```

An example is shown here, where we upload a java file, **WhereAmI.java** to the UCSD school server. 
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/UploadingToLinuxServer.png)

After entering our password, the file will be uploaded to the server. 

Continuing from our previous example, we can now use the file uploaded in the remote server. We must first login using the **ssh** command. Then, we can use the **javac** command to compile the java file we just uploaded. Lastly, we can run the class file using the **java** command.
![Image](https://projectsbykyle.github.io/cse15l-lab-reports/RunningJavaOnServer.png)

The file upload progress is displayed on the right.


Now, We can use the **scp** command easily move files from our local computer to a remote server. 

## Setting an SSH Key
One annoying part of the **ssh** and **scp** commands is that we have to enter a password in everytime we want to access the remote server. This will be impractical and frustrating if we have to access the remote server often. 

Luckily, there is a simple solution to this. We can generate **ssh-keys** that will automatically take care of this everytime we access the remote server. 

On our local computer, we can run the following command. 

```
ssh-keygen
```
This generates a pair of **ssh keys**, one public and one private. The **public key** can be copied to some directory on the remote server, and we can keep the **private key** on our own computer. 

Now, the **ssh** command will use these 2 pairs of keys for authentication instead of asking for your password everytime. 

After running the command, you will be prompted to enter a directory to store the keys. Press **enter** to select the default one. 

An example output of this is shown here.

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/GeneratingSSHKey.png)

If you are using Windows, you may need to run some extra commands to finish setting this up. The instructions are [here](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation). 

Now that we have the two keys generated, we have to actually set them up the **ssh** and **scp** commands do our login work for us.

First, we need to make a **.ssh** directory on the server to store the **scp keys**. 

To begin, we first **ssh** to the remote server (just like how we did in the **Remotely Connecting** part of this tutorial).  
```
ssh username@server
```
We can then use the **mkdir** command to make the **.ssh** directory. **mkdir** does what it sounds like it does: **make** (mk) a **directory** (dir). 
```
mkdir .ssh
```
Now, we can log out using **exit**.  

Then, we can finally copy the public key to the remote server using the **scp** command. 
```
scp <your ssh-key directory> user@server:~/.ssh/authorized_keys

Example: scp /Users/studentA/.ssh/id_rsa.pub user@student.edu:~/.ssh/authorized_keys
```

Now, we can use **scp** and **ssh** without logging in!

## Optimizing Remote Running
Here are some tips that can help you make your remote running more smooth. 

One example would be adding a command at the end of the **ssh** command. The command you add will directly run on the remote server. 

For example:
```
ssh user@server "ls" 
```
This runs the **ls** command on the server. An example output is given below. 

![Image](https://projectsbykyle.github.io/cse15l-lab-reports/LSOnRemoteServer.png)

Another tip that you can use in general is combining multiple commands on the same line. We just seperate them using semicolons: 

```
cp File1.txt File2.txt; javac ARandomJavaFile.java; java ARandomJavaFile; cat ~/A/Directory/Path.txt
```

## Conclusion
The main takeaways from this tutorial are the **ssh** and **scp** commands. 

The **ssh** command allows us to login and access remote servers, and the **scp** commmand lets us copy files from our local computer to the remote server. 

These commands will be very useful as we work on larger projects using remote servers in the future. 

