# 2420-Assignment1

by the end of this guide users should be able to:

Create SSH keys on your local machine

Create a Droplet running Arch Linux using the `doctl` command-line tool.

use `doctl` and cloud-init to set up an Arch Linux droplet


# Create SSH keys on your local Machine:
SSH stands for Secure Shell, and it's a method of sending commands securely to another computer over an unsecured network. It is commonly used to control servers remotely, manage infrastructure, and transfer files. SSH uses Public key cryptography, a way to encrypt data with two different keys, a private key and a public key. Essentially, whenever someone wants to connect to a server via SSH, they use the server's public key to encrypt the communication. Only the server with its private key can decrypt it. This process makes it possible to communicate over an unsecured network as the data remains encrypted. [^1]

### WHY SSH? 
Before SSH,  administrators used Telnet, which was a method of sending data to another computer without encrypting the information; this made it prone to security-related threats. There are no authentication policies or data encryption techniques for telnet, which is why it is a huge security risk and another reason why it's less popular since SSH came out. Although it is used in private networks still, it's not used over unsecured or public networks.[^2] 


# Installing doctl

# Cloud-init

# deploying the droplet


# references:
