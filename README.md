# 2420-Assignment1

by the end of this guide users should be able to:

Create SSH keys on your local machine

Create a Droplet running Arch Linux using the `doctl` command-line tool.

use `doctl` and cloud-init to set up an Arch Linux droplet


# Create SSH keys on your local Machine:
SSH stands for Secure Shell, and it's a method of sending commands securely to another computer over an unsecured network. It is commonly used to control servers remotely, manage infrastructure, and transfer files. SSH uses Public key cryptography, a way to encrypt data with two different keys, a private key and a public key. Essentially, whenever someone wants to connect to a server via SSH, they use the server's public key to encrypt the communication. Only the server with its private key can decrypt it. This process makes it possible to communicate over an unsecured network as the data remains encrypted. [^1]

### WHY SSH? 
Before SSH,  administrators used Telnet, which was a method of sending data to another computer without encrypting the information; this made it prone to security-related threats. There are no authentication policies or data encryption techniques for telnet, which is why it is a huge security risk and another reason why it's less popular since SSH came out. Although it is used in private networks still, it's not used over unsecured or public networks.[^2] 

### creating the SSH keys:

1. Open the Terminal.
2.  Input:  

```bash
ssh-keygen -f ~/.ssh/my-key -C "Your-Email-Address"
```
```ssh-keygen``` is the command which will generate the SSH keys for you.
```-f``` is a flag or option which stores the SSH keys in the location you specify
```~``` is the current users home directory.
```-C``` is also a flag which lets you add a comment it is recommended to put something like your name or email address.

Note: ```ssh-keygen``` has multiple encryption method but we will stick with the default which is ed25519.[^3]

After running this command you will be asked for a passphrase. It is good practice to set a passphrase because if someone gets your private key they can imitate you. Not setting a passphrase would also mean that you trust the root user since they can get into any file with root privileges.

3. Input: 
 ```bash
 cd ~/.ssh
 ```
 ```cd``` stands for change directory.[^4]
 
 1. Input:
 ```bash
ls
```
```ls``` stands for list directory contents.[^5]

if everything went according to plan you should see something like this:

![[sshkeysmade.png]]
once we have our keys we now need to install ``` doctl``` in order to create a droplet
# Installing doctl

# Cloud-init

# deploying the droplet


# references:
