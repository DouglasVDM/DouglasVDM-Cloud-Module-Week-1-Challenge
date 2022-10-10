# DouglasVDM-Cloud-Module-Week-1-Challenge

## Setting up an Ubuntu Virtual Machine to allow for remote access

# Week_1

## 2.) Individual assignment:
    - Create an ubuntu VM on one of the main cloud providers. 
        If you have no preference, go with AWS.
    - Able to access your VM remotely.
    - Create a 2nd user that can also access the VM with a separate credential.
    - *Slack message* Berkeli Halmyradov. You should be able to SSH or SFTP to the VM
    - Use **cowsay** to set a welcome back message for the users every time they log into the server.
    - Advance: Secure your VM access to from your laptop only.
    - Advance: Setup anything useful on the server that you can showcase to the class.
    
    ### Important notes:
    - When setting up your cloud account, make sure you configure the MFA security right away.
    - AWS - when launching resources, make sure you are have chosen the 'free-tier' ones.
    - Keep an eye on the billing monitor.


# Day 01

1. Started [AWS Launch a Linux Virtual Machine](https://aws.amazon.com/getting-started/launch-a-virtual-machine-B-0/) tutorial
   - I have an AWS account setup with MFA
2. I was trying to understand **key pairs**. Did some digging and found AWS [Creating, displaying, and deleting Amazon EC2 key pairs](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-keypairs.html#creating-a-key-pair) doc
3. After reading up on key pairs, I moved on to setting up of my Linux instance. First I read [Amazon EC2 key pairs and Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
4. I'm enjoying the process of reading documentation.[Use EC2 Instance Connect to provide secure SSH access to EC2 instances with private IP addresses](https://aws.amazon.com/blogs/security/use-ec2-instance-connect-to-provide-secure-ssh-access-to-ec2-instances-with-private-ip-addresses/) seems to be a good overview on what is required.

# Day 02
1. Went through [How to connect to AWS ec2 instance from Ubuntu terminal](https://www.how2shout.com/linux/how-to-connect-to-aws-ec2-instance-from-ubuntu/) blog to connect.
2. ssh: connect to host 52.31.72.40 port 22: Connection timed out
3. Will revisit connecting to my instance later today by going through [I'm receiving "Connection refused" or "Connection timed out" errors when trying to connect to my EC2 instance with SSH. How do I resolve this?
](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-linux-resolve-ssh-connection-errors/)

# Day 03
1. Signed in to AWS with IAM user account.
2. A while ago, I setup my AWS account with MFA (multi-factor authentication), when I started studying for AWS Cloud Practitioner Exam (few months ago).
3. 1st Spent some reading the [Best Practices](https://docs.aws.amazon.com/accounts/latest/reference/best-practices.html)
4. Then stepped through Launching an Instance
5. After launching the ubuntuVM instance, I ticked the box to select the ubuntu instance, clicked on actions, next clicked on SSH Client tab and followed the steps ![image](https://user-images.githubusercontent.com/74470226/194226268-2fe63830-101f-4d33-90fe-d130fc03e903.png)
7. To connect from my local PC to my ubuntuVM on AWS, I followed these steps ![image](https://user-images.githubusercontent.com/74470226/194220883-c81a4916-0031-40e3-938b-6e4469ea7c22.png)
    * Firstly, I changed my directory to where my .pem file is stored ```cd Downloads```
    * To connect to ubuntuVM, I ran ```sh -i "ubuntuVM.pem" ubuntu@ec2-54-89-187-240.compute-1.amazonaws.com``` in the terminal
    * **ubuntu@ip-172-31-92-136:~$** indicates I've connected to ubuntuVm via SSH Client
9. Managed to connect to my instance via SSH client(Terminal) ![Connected to ubuntuVM instance via SSH Client](https://user-images.githubusercontent.com/74470226/194221550-a9a92088-71ae-434e-a203-a8ab83fd105b.png)
10. Checked my ubuntuVM Instance Status on AWS ![instance status](https://user-images.githubusercontent.com/74470226/194222474-9a67d28b-5b1e-4670-aa3f-2891255d6f0f.png)
11. How to add welcome message with ```cowsay```
    * Followed [Cool Custom Welcome Messages on Linux terminal](https://www.geeksforgeeks.org/cool-custom-welcome-messages-linux-terminal/) blog post to implement my welcome message
    * Started by installing fortune (prints out a random interesting proverb) and cowsay (displays a speaking cow in terminal window) by running ```sudo apt-get install fortune cowsay``` in the terminal
    * Next, opened the terminal to open the ./bashrc file using any editor of your choice. (Here, vim is used) ``` vim ~/.bashrc ```
    * Followed by adding a small line at the beginning of ~/.bashrc file, 
    * Saved the file and exit: Pressed “Esc” to exit this mode. Then typed “:w” to save my script. After that typed :wq to exit Vim
    * Finally typed ```exit``` to stop my instance then connected to my instance again by running the command in step 7 above
    * ![cowsay-welcome-message](https://user-images.githubusercontent.com/74470226/194425991-90f4c572-0865-4b6d-b36d-d53dd7675fbf.png)

# Day 04
1. I couldn't connect to my ubuntu instance ``` ssh -i "ubuntuVM.pem" ubuntu@172.31.92.136 
ssh: connect to host 172.31.92.136 port 22: Connection timed out ```
Something about **SSM Agent isn't installed on the instance.**
2. ![image](https://user-images.githubusercontent.com/74470226/194650896-6f575830-2249-42d6-acf4-348dae1525ed.png)
3. Working my through [Working with SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html)
4. Didn't have time to step through this documentation, I'll try some other time

# Day 05
1. I need to create a 2nd user with SSH access to my Amazon ubuntuVM instance. [How do I add new user accounts with SSH access to my Amazon EC2 Linux instance?](https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/) looks like a good 1st try documentation to attempt this challenge.
2. Every AWS EC2 Linux instance comes with a **default system user account**. Creating accounts for **New users** is much more secure than granting multiple users access to the default EC2 User Account. The default EC2 User Account can cause a lot of damage when used improperly.   to use their own files and workspaces
3. [How to add a new user to the system as well as providing remote access to the new user](https://youtu.be/khPGZYh73fo)
4. Connect to the Linux instance that you want to add new user on
5. Use the ```sudo adduser jd_douglas``` command, followed by the name of the user you want to ad. This adds an entry in ```/etc/password``` file, and creates a ```jd_douglas``` group, and creates a home directory for the new account in the ```/home directory```
6. To provide remote access to the ```jd_douglas``` account, create a ```.ssh``` directory in ```jd_douglas```'s ```home``` directory then create a file ```authorized_keys``` in ```.ssh``` directory which will later hold the ```public key```
7. Switch to new user by using ```sudo su - jd_douglas``` to switch to jd_douglas directory so that newly created files have the proper ownership. 
    > The cmd promp now says jd_douglas, showing that the SSH session has been swithched to the new account
8. To provide remote access to the jd_douglas user, create the new key pair for user jd_douglas
9. Navigate to the ```AWS EC2 Console```, left menu, under ```Network and Security``` click on ```Key Pairs``` then ```Create Key Pair```, next add a ```Key pair name``` = ```***********-keypair``` , click Create, save the file to a secure location
> This is the only time to **SAVE** the file!!!
10. ```mkdir .ssh```
11. ```chmod 700 .ssh``` to change the file permissions of .ssh directory to allow ```only``` the ```file owner``` to ```READ, WRITE or OPEN``` the directory
12. ```cd .ssh``` to change into the .ssh directory
13. ```touch authorized_keys``` to create keys file
14. ```chmod 600 authorized_keys``` to change the file permissions to allow ```only``` the ```file owner``` to ```READ or WRITE``` to the file
15. Retrieve the ```public key``` for the key pair
16. Open a new tab in terminal
17. ```chmod 400 ***********-keypair.pem``` to set the file permissions so that ```only``` the ```file owner``` can ```READ``` the file
18. ```ssh-keygen -y``` command to retrieve the public key
19. Enter the path for the key
20. Copy the key then move to the other terminal session
21. ```vim authorized_keys``` to open authirized_keys file
22. Paste the key in the file
23. Test loggin into jd_douglas account using the private key
24. ```ssh -i ***********-keypair.pem jd_douglas@ecs-***your public IP address***.compute-1.amazonaws.com```
25. ```id``` shows the user and group information jd_douglas, the current user

# Notes on the challenges
