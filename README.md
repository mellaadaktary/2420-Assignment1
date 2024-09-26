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
```ls``` stands for list directory contents.[^4]

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
- `init` initializes the authentication process[^8]

after running this command doctl will ask for the personal access token

11. paste it into the prompt and press enter

after this step run:
```bash
doctl account get
```
- `account get` retrieves details from your profile such as Email Address, Team, Account Droplet limit , and more. [^9]
if everything went successfully you should see an output that says this 
![[doctl account get.png]]

we are now going to connect the public SSH key from the previous task to our DigitalOcean account by running:[^7]
```bash
 doctl compute ssh-key import <name> --public-key-file ~/.ssh/my-key.pub
```
- `doctl compute ssh-key import` adds a new SSH key to your account
- `--public-key-file <filepath>` is a flag which needs the public ssh key file

To confirm that our ssh key is paired to our DigitalOcean account we can run:
```bash
doctl compute ssh-key list
```
if everything went successfully you should see something like this:
![[digitaloceanssh.png]]
#### Explanation

# Cloud-init
When trying to manage and configure multiple cloud instances and servers, creating them 1 by 1 can be very time-consuming. This is where Cloud-init comes into play, Cloud-init is an open-source initialization tool that was designed to make getting your systems up and running easy and configured to your liking.

When you deploy a new cloud instance, cloud-init takes the configuration that you gave it and automatically applies it like a checklist. Cloud-init can do a multitude of things such as set Hostnames, configure network interfaces, create user accounts, and even run scripts.[^10]

1. we need to create a cloud-init-ALinux.yml file:
```bash
vim cloud-init-ALinux.yml
```
now that we have the file paste this yml file content:
```yml
#cloud-config
users:
  - name: <your-username>
    primary_group: <Group-name>
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-ed25519 <Public key> <your-email>
packages:
  - ripgrep
  - rsync
  - neovim
  - fd
  - less
  - man-db
  - bash-completion
  - tmux
disable_root: true
```

#### explanations of everything above
how to get public key

Note: make sure you are in insert mode

# deploying the droplet

we need to check for our custom image[^11] using the command below:
```bash
doctl compute image list-user
```

you should see something like this:
![[custom-image.png]]

now we can create the droplet[^12] by typing:
```bash
doctl compute droplet create <dropletname> --image <Custom-image-id> --region sfo3 --size s-1vcpu-1gb --ssh-keys <SSH Key ID> --user-data-file <Path-to-cloud-init-ALinux.yml> 
```

show success page
![[Pasted image 20240926095958.png]]

### explanations ^

to connect to ur ssh 
```bash
ssh -i ~/.ssh/my-key usercloudinitFile@<KEYID>
```

```bash
doctl compute ssh-key list
```

gives list of ssh keys on digital ocean account
dosentwork^



# references:
[^1]: https://www.cloudflare.com/learning/access-management/what-is-ssh/ 
[^2]: https://www.geeksforgeeks.org/difference-ssh-telnet/ 
[^3]: https://wiki.archlinux.org/title/SSH_keys
[^4]: https://explainshell.com/explain?cmd=ls
[^5]: https://docs.digitalocean.com/reference/doctl/#:~:text=doctl%20allows%20you%20to%20interact,clusters%2C%20domains%2C%20and%20more.
[^6]: https://docs.cloud-init.io/en/latest/explanation/introduction.html
[^7]: https://docs.digitalocean.com/reference/doctl/reference/compute/ssh-key/import/
[^8]: https://docs.digitalocean.com/reference/doctl/reference/auth/init/
[^9]: https://docs.digitalocean.com/reference/doctl/reference/account/get/
[^10]: https://docs.cloud-init.io/en/latest/explanation/introduction.html
[^11]: https://docs.digitalocean.com/reference/doctl/reference/compute/image/list/
[^12]: https://docs.digitalocean.com/reference/doctl/reference/compute/droplet/create/