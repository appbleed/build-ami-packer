# Create Amazon Machine Image (AMI) using Packer

### Authored by AppBleed (Open Source)

Appbleed provides a set of DevOps enabler ready to use tools.

# Objective

  - Build Ubuntu AMI using Packer and provision software 
  - Build Windows 2016 AMI using Packer and provision software
  - Create EC2 instance using the build images and test provision software

# Prerequisites

  - Packer is installed and you know what it does
  - You have an active AWS account (Free tier account is fine)
  - git is installed
  - Understand JSON file structure
  - Basics of bash command
  - Basics of powershell

Packer is a tool for creating identical machine images for multiple platforms from a single source configuration. What is [packer](https://packer.io)? How to  [install](https://www.packer.io/intro/getting-started/install.html)? Learn Packer [template](https://www.packer.io/docs/templates/index.html).

### File Structure

Read below section to understand the purpose of each file in this repo:

| FileName | Purpose |
| ------ | ------ |
| ubuntu-ami.json | A json file which is used by packer to build ubuntu based AMI |
| windows-ami.json | A json file which is used by packer to build windows based AMI |
| scripts/ubuntu | Folder to keep provisioning bash script and dependencies for ubuntu OS |
| scripts/windows | Folder to keep provisioning Powershell script and dependencies for windows OS |
| scripts/ubuntu/bootstrap.sh | bash script to povision softwares in image |
| scripts/windows/bootstrap.ps1 | Powershell script to povision softwares in image |

### Create ubuntu AMI

Make sure packer is installed and it's path is set by running packer command
```sh
$ packer
```
Make sure git is installed and then clone this repo
```sh
$ git
$ git clone https://github.com/appbleed/build-ami-packer.git
$ cd build-ami-packer
```

Run below command to create ubuntu AMI
```sh
$ packer validate unbuntu-ami.json
```
You should see validation output
```sh
Template validated successfully.
```

Run below command to create ubuntu AMI

```sh
$ packer build unbuntu-ami.json
```
Supply your AWS aws_access_key and aws_secret_key in prompt.

This will create a new EC2 instanace based on the source_ami, does the software provisioning, stops the instance, creates an AMI based on new instance and then terminates the EC2 instance.

### Output

```sh
C:\Projects\AppBleed\DevOps\build-ami-packer>packer build ubuntu-ami.json
amazon-ebs output will be in this color.

==> amazon-ebs: Prevalidating AMI Name: ubuntu-aws-base-1527489477
    amazon-ebs: Found Image ID: ami-963cecf4
==> amazon-ebs: Creating temporary keypair: packer_5b0ba3c6-c57a-c000-adad-368415bd943d
==> amazon-ebs: Creating temporary security group for this instance: packer_5b0ba3d0-a168-d641-c8e2-e044043276eb
==> amazon-ebs: Authorizing access to port 22 from 0.0.0.0/0 in the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
==> amazon-ebs: Adding tags to source instance
    amazon-ebs: Adding tag: "Name": "Packer Builder"
    amazon-ebs: Instance ID: i-0f604c2b9f453f5ac
==> amazon-ebs: Waiting for instance (i-0f604c2b9f453f5ac) to become ready...
==> amazon-ebs: Waiting for SSH to become available...
==> amazon-ebs: Connected to SSH!
==> amazon-ebs: Uploading ./scripts/ubuntu/welcome.txt => /home/ubuntu/
==> amazon-ebs: Provisioning with shell script: C:\Users\shravan.jha\AppData\Local\Temp\packer-shell843963935
    amazon-ebs: total 32
    amazon-ebs: drwxr-xr-x 4 ubuntu ubuntu 4096 May 28 06:38 .
    amazon-ebs: drwxr-xr-x 3 root   root   4096 May 28 06:38 ..
    amazon-ebs: -rw-r--r-- 1 ubuntu ubuntu  220 Aug 31  2015 .bash_logout
    amazon-ebs: -rw-r--r-- 1 ubuntu ubuntu 3771 Aug 31  2015 .bashrc
    amazon-ebs: drwx------ 2 ubuntu ubuntu 4096 May 28 06:38 .cache
    amazon-ebs: -rw-r--r-- 1 ubuntu ubuntu  655 May 16  2017 .profile
    amazon-ebs: drwx------ 2 ubuntu ubuntu 4096 May 28 06:38 .ssh
    amazon-ebs: -rw-rw-r-- 1 ubuntu ubuntu   18 May 28 06:38 welcome.txt
    amazon-ebs: WELCOME TO PACKER!
==> amazon-ebs: Provisioning with shell script: ./scripts/ubuntu/bootstrap.sh
    amazon-ebs:  ***You can add bash scripts to install prerequisite software in you image in bootstrap.sh file ***
==> amazon-ebs: Stopping the source instance...
    amazon-ebs: Stopping instance, attempt 1
==> amazon-ebs: Waiting for the instance to stop...
==> amazon-ebs: Creating the AMI: ubuntu-aws-base-1527489477
    amazon-ebs: AMI: ami-705f8c12
==> amazon-ebs: Waiting for AMI to become ready...
==> amazon-ebs: Terminating the source AWS instance...
==> amazon-ebs: Cleaning up any extra volumes...
==> amazon-ebs: No volumes to clean up, skipping
==> amazon-ebs: Deleting temporary security group...
==> amazon-ebs: Deleting temporary keypair...
Build 'amazon-ebs' finished.

==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:
ap-southeast-2: ami-705f8c12
```
#### Validate Result
Login to aws console. 
Go to Services>EC2>Images>AMIs. Make sure you select the right aws region at top right corner.
You should see your newly created AMI with AMI name starts with ubuntu-aws-base.

### Create windows AMI
Make sure you have packer and git installed and you have already cloned this repo.

Run below command to create ubuntu AMI
```sh
$ cd build-ami-packer
$ packer validate windows-ami.json
```
You should see validation output
```sh
Template validated successfully.
```

Run below command to create windows AMI

```sh
$ packer build windows-ami.json
```
Supply your AWS aws_access_key and aws_secret_key in prompt.

This will create a new EC2 instanace based on the source_ami, does the software provisioning, stops the instance, creates an AMI based on new instance and then terminates the EC2 instance.

### Output

```sh
C:\Projects\AppBleed\DevOps\build-ami-packer>packer build windows-ami.json
amazon-ebs output will be in this color.

==> amazon-ebs: Prevalidating AMI Name: windows-aws-base-1527493663
    amazon-ebs: Found Image ID: ami-c971a6ab
==> amazon-ebs: Creating temporary keypair: packer_5b0bb41f-ade9-6642-1f75-4ceccd1076b4
==> amazon-ebs: Creating temporary security group for this instance: packer_5b0bb425-bf0a-4fbd-ff3a-1968debd5aa8
==> amazon-ebs: Authorizing access to port 5985 from 0.0.0.0/0 in the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
==> amazon-ebs: Adding tags to source instance
    amazon-ebs: Adding tag: "Name": "Packer Builder"
    amazon-ebs: Instance ID: i-0d8680ea0547f3729
==> amazon-ebs: Waiting for instance (i-0d8680ea0547f3729) to become ready...
==> amazon-ebs: Skipping waiting for password since WinRM password set...
==> amazon-ebs: Waiting for WinRM to become available...
    amazon-ebs: WinRM connected.
==> amazon-ebs: Connected to WinRM!
==> amazon-ebs: Provisioning with Powershell...
==> amazon-ebs: Provisioning with powershell script: C:\Users\shravan.jha\AppData\Local\Temp\packer-powershell-provisioner682797843
    amazon-ebs: HELLO NEW USER; WELCOME TO PACKER
    amazon-ebs: You need to use backtick escapes when using
    amazon-ebs: characters such as DOLLAR$ directly in a command
    amazon-ebs: or in your own scripts.
==> amazon-ebs: Restarting Machine
==> amazon-ebs: Waiting for machine to restart...
    amazon-ebs: WIN-P2694GMJUT9 restarted.
==> amazon-ebs: Machine successfully restarted, moving on
==> amazon-ebs: Provisioning with Powershell...
==> amazon-ebs: Provisioning with powershell script: ./scripts/windows/bootstrap.ps1
    amazon-ebs: PACKER_BUILD_NAME is an env var Packer automatically sets for you.
    amazon-ebs: ...or you can set it in your builder variables.
    amazon-ebs: The default for this builder is: amazon-ebs
    amazon-ebs: The PowerShell provisioner will automatically escape characters
    amazon-ebs: considered special to PowerShell when it encounters them in
    amazon-ebs: your environment variables or in the PowerShell elevated
    amazon-ebs: username/password fields.
    amazon-ebs: For example, VAR1 from our config is: A$Dollar
    amazon-ebs: Likewise, VAR2 is: A`Backtick
    amazon-ebs: VAR3 is: A'SingleQuote
    amazon-ebs: Finally, VAR4 is: A"DoubleQuote
    amazon-ebs: None of the special characters needed escaping in the template
==> amazon-ebs: Stopping the source instance...
    amazon-ebs: Stopping instance, attempt 1
==> amazon-ebs: Waiting for the instance to stop...
==> amazon-ebs: Creating the AMI: windows-aws-base-1527493663
    amazon-ebs: AMI: ami-4b5c8f29
==> amazon-ebs: Waiting for AMI to become ready...
==> amazon-ebs: Terminating the source AWS instance...
==> amazon-ebs: Cleaning up any extra volumes...
==> amazon-ebs: No volumes to clean up, skipping
==> amazon-ebs: Deleting temporary security group...
==> amazon-ebs: Deleting temporary keypair...
Build 'amazon-ebs' finished.

==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:
ap-southeast-2: ami-4b5c8f29
```
#### Validate Result
Login to aws console. 
Go to Services>EC2>Images>AMIs. Make sure you select the right aws region at top right corner.
You should see your newly created AMI with AMI name starts with windows-aws-base.

### Todos

 - Use the created base AMI to create another AMI with developers tools installed in it
 - Create a Vargant box for developer based on above newly created image.

License
----

MIT


**Free Software, Hell Yeah!**
