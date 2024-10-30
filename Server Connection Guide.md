# PyCharm Remote Connection Guide

| Server Basic Information |                    |
| ------------------------ | ------------------ |
| Host 1                   |                    |
| Host 2                   |                    |
| Port                     |                    |
| Username                 | ps (I'm a manager) |
| Password                 |                    |



## 1 Preparation

**3 tasks:** 

- download PyCharm Professional Edition

- create a personal user account in the Linux system

- create a personal conda virtual environment



#### 1.1 Download PyCharm Professional Edition

The PyCharm Community Edition **does not support SSH remote connection**. Apply for the [PyCharm Professional Edition download link](https://zhuanlan.zhihu.com/p/338280181?utm_id=0) with your educational email for free.




#### 1.2 Create a personal user account in the Linux system and add it to the Anaconda group

> Why create a personal user account in the Linux system?
>
> To maintain system security and personal privacy, while avoiding confusion in a shared environment (since the server has many shared users, we should keep things organized to protect personal data).

open the project in PyCharm
open the terminal
click the dropdown button
select `New SSH Session`

<img src=".\images-en\en1.png" width="90%" height="auto">

Connect to the remote server as the ps user

<img src=".\images-en\en2.png" width="40%" height="auto">

Successful login as the ps user

<img src=".\images-en\en3.png" width="70%" height="auto">

Begin by creating your own user account, using `aaa` as an example; replace it with your initials for your username

```
sudo adduser username # Add new user
sudo usermod -aG anaconda username # Allow the new user to use the system anaconda
id username # View user information
```

Note: Linux usernames can only use **lowercase letters, numbers, and underscores**.

<img src=".\images-en\en4.png" width="50%" height="auto">

##### Note: What if I make a mistake?  Just delete the user and start over.

```
userdel –r aaa # Delete user aaa and its home directory
```



#### 1.3 Create a personal conda virtual environment in the Linux system

> Choose **Create an empty environment** for a fresh, clean setup.
>
> Choose **Clone an existing environment** if you find someone else's environment suitable for you.
>
> If you find it too cumbersome, you can choose to **Copy an existing environment**.



##### 1.3.1 Create an empty environment

```
su root # Switch to root user, password is the same as ps user
conda create -n your_env_name python=3.9 # You can choose another Python version
```

<img src=".\images-en\en5.png" width="60%" height="auto">

<img src=".\images-en\en6.png" width="60%" height="auto">

<img src=".\images-en\en7.png" width="60%" height="auto">



##### 1.3.2 Clone an existing environment

```
su root # Switch to root user, password is the same as ps user
conda create -n new_env_name --clone existing_env_name
```



##### 1.3.3 Copy an existing environment (by Lin Chen)

1. Enter the Anaconda3 environment directory:

   ````shell
   ## Host 1
   cd  /usr/local/anaconda3/envs
   ## Host 2
   cd /opt/anaconda3/envs
   ````

2. Find the desired environment to copy

   ````````shell
   ls
   ````````

3. Perform the copying

   ```````````shell
   cp -r $A $B   
   ## $A is the directory name you want to copy
   ## $B is your environment name; you will activate it with this name later
   ```````````

4. Set permissions

   ````````````shell
   chmod 777 $B
   ## Same as $B in the third step
   ````````````



##### Supplement: Common Conda Commands Reference

|         Operation         |                          Command                          |
| :-----------------------: | :-----------------------------------------------------: |
|       Create Environment   |        conda create -n your_env_name python=3.9        |
|       Clone Environment    | conda create -n new_env_name --clone existing_env_name |
|       Activate Environment  |              conda activate your_env_name              |
|       Deactivate Environment|                    conda deactivate                    |
|       Remove Environment    |           conda env remove -n your_env_name            |
|   List All Installed Envs   |             conda env list / conda info -e             |
|        Install Package      | conda install package_name / pip install package_name  |
|        Uninstall Package    |  conda remove package_name / pip remove package_name   |
|   List All Installed Packages|                 conda list / pip list                  |



## 2 PyCharm SSH Remote Connection⭐⭐⭐

**3 tasks:** 

- configure the remote Python interpreter

- set up the remote code storage path

- upload local files to the remote server

> It’s advisable to configure the interpreter before establishing a connection to the server.
>
> The difference will be evident after a try.

**4 essential operations to master:** 

- Settings - Python Interpreter

- Tools - Deployment - Configuration

- Tools - Deployment - Browse Remote Host

- Right-click on project files - Deployment - Upload to...

**One important prerequisite: ensure that your local machine and the server are on the same local network; otherwise, the SSH connection will fail (remember to connect to the campus network in your dorm).**



#### 2.1 Configure Remote Python Interpreter

`File - Settings - Project - Python Interpreter - Add Interpreter - SSH` - Connect to the server - Select the Python interpreter from your personal env

<img src=".\images-en\en8.png" width="95%" height="auto">

<img src=".\images-en\en9.png" width="40%" height="auto">

<img src=".\images-en\en10.png" width="40%" height="auto">

<img src=".\images-en\en11.png" width="40%" height="auto">

<img src=".\images-en\en12.png" width="70%" height="auto">

<img src=".\images-en\en13.png" width="70%" height="auto">

<img src=".\images-en\en14.png" width="70%" height="auto">



#### 2.2 Deploy Remote Code Storage Path

`Tools - Deployment - Configuration`

<img src=".\images-en\en15.png" width="90%" height="auto">

At this point, you will see this interface. **Edit the root path to `/lab_202/username`**

<img src=".\images-en\en16.png" width="70%" height="auto">

Then, modify the **deployment path** in the mappings.

<img src=".\images-en\en17.png" width="70%" height="auto">

<img src=".\images-en\en27.png" width="70%" height="auto">

After completing sections 2.1 and 2.2, you should see the following interface

displaying information for **Remote Server** and **Remote Python Interpreter**

<img src=".\images-en\en18.png" width="100%" height="auto">



#### 2.3 View Remote Host Files in PyCharm

`Tools - Deployment - Browse Remote Host`

<img src=".\images-en\en19.png" width="90%" height="auto">

A small icon will appear on the right, showing the files in the **root directory**

You might find that the `aaa` folder does not exist under `lab_202`

Solution: 

Open the terminal

connect remotely as user `aaa`

create `aaa/projects` in the `lab_202` path

<img src=".\images-en\en29.png" width="90%" height="auto">

<img src=".\images-en\en30.png" width="90%" height="auto">

The program failed to run because the local code has not yet been uploaded

<img src=".\images-en\en31.png" width="80%" height="auto">

It's a minor issue

see section 2.4 to resolve it by uploading the code



#### 2.4 Upload Local Files to Remote Server

For the initial upload, you need to upload the entire project. Click to select the `deleteit` project, right-click - `Deployment - Upload to aaa@...` .

Subsequently, you can also upload individual small files from the project: Click to select `a.py`, right-click - `Deployment - Upload to aaa@...`.

<img src=".\images-en\en32.png" width="70%" height="auto">

<img src=".\images-en\en33.png" width="80%" height="auto">

Run the program

And there you go—it works!

<img src=".\images-en\en34.png" width="90%" height="auto">



## 3 FAQ



#### How to Install Packages in Personal Conda Virtual Environment?

In the PyCharm terminal, connect to the server and install the package:

```
source activate aaa
pip install numpy
```



#### Specify GPU

```python
device = torch.device("cuda:1") # {0, 1, 2, 3}
```


#### Check GPU Status

Open the terminal, connect to the server, and type:

```
nvidia-smi
```

To update every 2 seconds:

```
watch -n 2 nvidia-smi
```

To update every 2 seconds and display only the last 30 lines:


```
watch -n 2 "nvidia-smi | tail -n 30"
```



#### Memory Not Released

Open the terminal,

connect to the server, and type:

`kill  ` PID

<img src=".\images-en\en35.png" width="90%" height="auto">

#### Check Process User

`ps -o user= -p ` PID

