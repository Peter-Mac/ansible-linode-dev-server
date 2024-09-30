# Ansible Dev Server Setup

This project helps set up a single devleopment server on Linode.
## Background
I use a long term Linode server as a test and learn palyground. I find it useful to have a non mission-critical server to which I can test out various tools and services on. Unfortunately, over time this box by its very nature is prone to tech drift as I look back at my command history, I can see the times where I've installed lots of experimental libraries, repositories and other software versions.
The idea with this project is to create the ability at any time to go back to basics, nuke the box and start playing all over again.

## The Setup
The setup consists of three parts
- Provisioning the server (provision.yml)
- Manual creation of ssh keys using ssh-keygen and copying to server using ssh-copy
- Configuring the server (setup.yml)
There is a bit of manual tweaking involved (for the moment) which is creating a pub/private key, and copying the public key to the server for continued access after the initial provisioning step.

**Pre-requisites are:**
- An active Linode account
- An API Personal Token (issued by Linode - requested/managed by account holder)
- The use of an ansible valut to store:
  - The Linode API Personal Token value
  - A root pasword to be used to access the server until the SSH key process is enabled
- working install of ansible (e.g. brew install ansible)

## Results
The running of the 2 playbooks (and the config of the SSH keys) will  create a relativelsy secure Ubuntu based server with passwordless SSH access for a single user 'ubuntu'.
The user's home folder has zsh installed and is ready for use with some typical dev tools. I didn't want this project to get too far into the details of installing and customising the kithen sink as everyone's needs are different. The intent here is to get the basic shell working. I'll customise the hell out of the install once the shell is up and running.

My approach with the ssh key is to create a public/private key pair in my home ~/.ssh folder.
Then use ssh-copy to copy the public part to the authorized_keys file on the server. This should be sufficient to get ssh up and running without passwords. A further step to perform for ssh hardening is to disable root access by applying changes to the ssh server config file ( /etc/sshd_config). This is performed as one of the final steps in the configure process.

```
ssh-keygen -t ed25519
...
...complete the key generation process...
...
...then...
ssh-copy-id -i ~/.ssh/linode_ed25519.pub root@<my-ip-address>
```
 
## Further TODOs
- Enable a multi-server run. This run is configured for one server. Would need to add a loop for some processing on a multi server setup.

- Add DNS setup so the server is allocated a proper name under my domain.

- Post install reporting to remind me to us e a specific SSH identity file.
