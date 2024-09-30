# 2420-Assignment1

By the end of this guide users will be able to:

- Create SSH keys on your local machine.

- Create a Droplet running Arch Linux using the `doctl` command-line tool.

- Use `doctl` and cloud-init to set up an Arch Linux droplet.


# Create SSH Keys on Your Local Machine:
SSH stands for Secure Shell, and it's a method of sending commands securely to another computer over an unsecured network. It is commonly used to control servers remotely, manage infrastructure, and transfer files. SSH uses Public key cryptography, a way to encrypt data with two different keys, a private key and a public key. Essentially, whenever someone wants to connect to a server via SSH, they use the server's public key to encrypt the communication. Only the server with its private key can decrypt it. This process makes it possible to communicate over an unsecured network as the data remains encrypted. [^1]

### WHY SSH? 
Before SSH,  administrators used Telnet, which was a method of sending data to another computer without encrypting the information; this made it prone to security-related threats. There are no authentication policies or data encryption techniques for telnet, which is why it is a huge security risk and another reason why it's less popular since SSH came out. Although it is used in private networks still, it's not used over unsecured or public networks.[^2] 

Note: SSH keys provide secure, password-less login to remote servers and will be essential for our droplet setup.
### Creating the SSH Keys:

1. Open the Terminal.
2.  Input:  

```bash
ssh-keygen -f ~/.ssh/my-key -C "Your-Email-Address"
```
- ```ssh-keygen``` is the command which will generate the SSH keys for you.
- ```-f``` is a flag or option which stores the SSH keys in the location you specify
- ```~``` is the current users home directory.
- ```-C``` is also a flag which lets you add a comment it is recommended to put something like your name or email address.

This command generates SSH key pairs in the specified directory with a comment inside the public key file

Note: ```ssh-keygen``` has multiple encryption methods but we will stick with the default which is ed25519.[^3]

After running this command you will be asked for a passphrase. It is good practice to set a passphrase because if someone gets your private key they can imitate you. Not setting a passphrase would also mean that you trust the root user since they can get into any file with root privileges.

3. Input: 
 ```bash
 cd ~/.ssh
 ```
 - ```cd``` stands for change directory.[^4]
 
 1. Input:
 ```bash
ls
```
- ```ls``` stands for list directory contents.[^4]

If everything went according to plan you should see something like this:

![[sshkeysmade.png]]

once we have our keys we now need to install ``` doctl``` in order to create the droplet
# Installing doctl
doctl is the official DigitalOcean command line interface that allows you to interact with the DigitalOcean API with the command line. Just like the control panel, you can create, configure, and destroy DigitalOcean resources such as Droplets using doctl. doctl allows us to create and manage DigitalOcean resources directly from the command line, making it easier to automate droplet creation[^5] 



To Install doctl we need to run:
1. 
``` bash
sudo pacman -S doctl
```
- `sudo` stands for super user do and it temporarily upgrades your privileges to run commands only the `root`user could execute
- `pacman` stands for package manager and is one of Arch Linux most distinguishing features it can do many things such as install, upgrade, and delete packages
- `-S` is the flag which lets us install packages with `pacman` along with the dependencies.
- `doctl` is what we are installing
After installing `doctl` run this command to make sure everything went as planned:
```bash
doctl version
```

- `version` ensures that the install went smoothly by showing us this in the terminal output:
![[Pasted image 20240928193242.png]]
# Creating API token
 A personal access token lets a user authenticate to a service to access or change the protected resources, these can be used as an alternative to passwords. In our case we need the PAT(Personal Access Token) to link our `doctl` to our Digital Ocean account.

   Note: you must use a browser capable of running JavaScript for this step

1.  Go to https://cloud.digitalocean.com/account/api/tokens
2. Click on Generate New Token
3. Input a Token Name
4. Leave the Expiration date to 90 days
5. Select Full Access on Scopes
6. Click Generate token
7. Copy the new personal access token it should look something like this:

![[personaltoken.png]]

10. Go back to your terminal and type: 

```bash 
doctl auth init
```
- `doctl` is the Command-Line Interface for DigitalOcean
- `auth` deals with authentication-related tasks
- `init` initializes the authentication process[^8]

After running this command doctl will ask for the personal access token

11. Paste it into the prompt and press enter

After this step run:
```bash
doctl account get
```
- `account get` retrieves details from your profile such as Email Address, Team, Account Droplet limit , and more. [^9]

If everything went successfully you should see an output that says this

![[doctl account get.png]]

We are now going to connect the public SSH key from the previous task to our DigitalOcean account by running:[^7]
```bash
 doctl compute ssh-key import <name> --public-key-file ~/.ssh/my-key.pub
```
- `doctl compute ssh-key import` adds a new SSH key to your account
- `--public-key-file <filepath>` is a flag which needs the public ssh key file

To confirm that our ssh key is paired to our DigitalOcean account we can run:
```bash
doctl compute ssh-key list
```
- `list` just shows all the SSH keys which are linked to your account
if everything went successfully you should see something like this:

![[digitaloceanssh.png]]

# Cloud-init
When trying to manage and configure multiple cloud instances and servers, creating them 1 by 1 can be very time-consuming. This is where Cloud-init comes into play, Cloud-init is an open-source initialization tool that was designed to make getting your systems up and running easy and configured to your liking.

When you deploy a new cloud instance, cloud-init takes the configuration that you gave it and automatically applies it like a checklist. Cloud-init can do a multitude of things such as set Hostnames, configure network interfaces, create user accounts, and even run scripts.[^10]

1. We need to create a cloud-init-ALinux.yml file:
```bash
nvim cloud-init-ALinux.yml
```
- `nvim` stands for Neovim which is a continuation and extension of Vim the linux text editor
here we are just using it to create the .yml file

Now that we have the file paste this yml file content:
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
- `name:` is for the username of user
- `primary_group` is for Group name(usually left as username)
- `groups:` specify which group you want user to be in
- `wheel` is the group which has access to `sudo` command
- `shell: /bin/bash` specifies the shell path
- `sudo: ['ALL=(ALL) NOPASSWD:ALL']` allows for sudo access
- `ssh-authorized-keys:` specify which SSH key you want to add for the user
- `ssh-ed25519` is the start of the key add the rest of it from the public key file
- `packages` installs all packages listed below
- `disable_root: true` disables root account on the cloud instance(its good practice to do this as it prevents unauthorized access to root account) 

Note: these packages are here for example reasons your YAML file can be completely different under the packages section feel free to customize the list.

# Deploying the droplet

We need to check for our custom image[^11] using the command below:
```bash
doctl compute image list-user
```
- `list-user` specifies it to your account

this command lists the all the private images on your account

you should see something like this:

![[custom-image.png]]

Now we can create the droplet[^12] by typing:
```bash
doctl compute droplet create <dropletname> --image <Custom-image-id> --region sfo3 --size s-1vcpu-1gb --ssh-keys <SSH Key ID> --user-data-file <Path-to-cloud-init-ALinux.yml> 
```
- `droplet create` creates a droplet on your account
- `--image` specifies the image we are gonna use to create the droplet
- `--region` specifies the region to create the droplet in
- `--size` indicates the size of the droplet
- `--ssh-keys` list of SSH key ids to embed in the droplets root access
- --user-data-file this contains our cloud-init file path

After running this command you should see a screen similar to this:

![[Pasted image 20240926095958.png]]
# Connecting to Droplet
```bash
ssh -i ~/.ssh/my-key usernamecloudinitFile@<IPv4-address>
```
- `-i` specifies which private key to use when connecting to the remote server
you can run the command below if you are not sure what your `IPv4` is.
```bash
doctl compute ssh-key list
```

#### If everything has gone according to plan, you should now be connected to your brand-new Droplet using `doctl` , `Cloud-init`, and SSH. 

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