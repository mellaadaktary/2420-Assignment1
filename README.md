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
once we have our keys we now need to install ``` doctl``` in order to create the droplet
# Installing doctl
doctl is the official DigitalOcean command line interface that allows you to interact with the DigitalOcean API with the command line. Just like the control panel, you can create, configure, and destroy DigitalOcean resources such as Droplets using doctl.[^5] 

to Install doctl we need to run:
1. 
``` bash
sudo pacman -S doctl
```
- `sudo` stands for super user do and it temporarily upgrades your privileges to run commands only the `root`user could execute
- `pacman` stands for package manager and is one of Arch Linux most distinguishing features it can do many things such as install, upgrade, and delete packages
- `-S` is the flag which lets us install packages with `pacman` along with the dependencies.
2. after running the previous command go to https://cloud.digitalocean.com/account/api/tokens
3. Click on Generate New Token
4. Input a Token Name
5. Leave the Expiration date to 90 days
6. Select Full Access on Scopes
- Note: we give full Access as we want doctl to be able to run the commands as the current users permission level
8. Click Generate token
9. Copy the new personal access token it should look something like this:
![[personaltoken.png]]
go back to your terminal and type:
10. 
```bash 
doctl auth init
```
- `doctl` is the Command-Line Interface for DigitalOcean
- `auth` deals with authentication-related tasks
- `init` initializes the authentication process

after running this command doctl will ask for the personal access token

11. paste it into the prompt and press enter

after this step run:
```bash
doctl account get
```
^explanation
if everything went successfully you should see an output that says this 
![[doctl account get.png]]

we are now going to connect the public SSH key from the previous task to our DigitalOcean account by running:[^7]
```bash
 doctl compute ssh-key import <name> --public-key-file ~/.ssh/my-key.pub
```
##### explanations

To confirm that our ssh key is paired to our DigitalOcean account we can run:
```bash
doctl compute ssh-key list
```
if everything went successfully you should see something like this:
![[digitaloceanssh.png]]
#### Explanation

# Cloud-init

# deploying the droplet


# references:
