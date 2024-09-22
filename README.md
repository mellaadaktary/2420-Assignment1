# 2420-Assignment1

# Tasks

create a tutorial using markdown


takes a user through the steps of creating a remote server using DigitalOcean

- Create ssh keys and add them to your digital ocean account

- add a custom arch linux image

- create a droplet running arch linux using the "doctl" command-line tool

- use `doctl` and cloud-init for every stage of setting up Arch Linux droplet

  

# INSTRUCTION DETAILS

    use a cloud-init configuration file to:

        1.create a regular user

        2.install some initial packages

        3.add a public ssh key to the authorized_keys file in your new users home directory

        4. disable root access via ssh

  
  
  
  
  
  
  

# Title(explain what ssh is and why its better than password authentication)

  

Create an SSH key pair

  

to create an SSH key pair you need to open your terminal and run the ```ssh-keygen```  command

  

Your computer should already ```ssh-keygen``` installed

open your terminal and run:

#### Linux and mac:
```ssh-keygen -t ed25519 -f ~/.ssh/do-key -C "name or email" ```


Note: windows users might need to create .SSH directory first.
#### Windows
```ssh-keygen -t ed25519 -f C:\Users\user-name\.shh\do-key -C "name or email" ```

# explanation of the commands and - and what everything does

-
-
-
-
-
-
-
-
-
-
-
note that ```ssh-keygen``` generates two keys
A. "do-key"a private key which is for you to keep
B. "do-key.pub" this is your public key that you copy to your server
# Adding public key to digital ocean

after making the ssh key pair you need to add the public key to your DigitalOcean account

to add the publick key pair to you account you need to copy it to the terminal.
the following commands show you how to copy the content to your clipboard from your terminal

In windows:
```Get-content C:\Users\user-name\.ssh\do-key.pub | Set-Clipboard```
In mac:
```pbcopy < ~/.ssh/do-key.pub```
In Linux:
```wl-copy < ~/.ssh/do-key.pub```
(Note! it can vary depending on your Linux system)

once you have the run the above command the ```do-key.pub``` should be copied to your clipboard now we need to use the CLI (command-line Interface) to edit our droplet
# Doctl

