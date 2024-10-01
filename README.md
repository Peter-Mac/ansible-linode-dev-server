# Ansible Dev Server Setup

This project helps set up one or more develeopment servers on Linode.
## Background
I use a long term Linode server as a test and learn palyground. I find it useful to have a non mission-critical server to which I can test out various tools and services on. Unfortunately, over time this box by its very nature is prone to tech drift as I look back at my command history, I can see the times where I've installed lots of experimental libraries, repositories and other software versions.
The idea with this project is to create the ability at any time to go back to basics, nuke the box and start playing all over again.

## The Setup
The setup consists of three parts
- Manual creation of ssh keys using ssh-keygen and configuring variables accordingly (ssh_key_name and ssh_key_dir). 
- Provisioning the server(s) (provision.yml)
- Configuring the server(s) (setup.yml)
There is a bit of manual tweaking involved (for the moment) which is creating a pub/private key. I tried this with the builtin ansible tasks, but kept failing with an invalid key.

**Pre-requisites are:**
- An active Linode account
- An API Personal Token (issued by Linode - requested/managed by account holder)
- The use of an ansible vault to store:
  - The Linode API Personal Token value
  - A root pasword to be used to access the server until the SSH key process is enabled
- working install of ansible (e.g. brew install ansible, or whatever works for your system)

## Results
The running of the 2 playbooks (and the config of the SSH keys) will create a relatively secure Ubuntu based server with passwordless SSH access for a single user 'ubuntu'.
If you want more than one server, simply add it as another node name under the linode_servers variable in the varibales file global_vars/main.yml

```
linode_servers:
  - name: "ansible-dev-01"
  - name: "ansible-dev-02"
```

The user's home folder has zsh installed and is ready for use with some typical dev tools. I didn't want this project to get too far into the details of installing and customising the kithen sink as everyone's needs are different. The intent here is to get the basic shell working. I'll customise the hell out of the install once the shell is up and running.

My approach with the ssh key is to create a public/private key pair in my home ~/.ssh folder.
Then, once the ubuntu user has been created, access to the server needs you to specify the key as per:

```

 use ssh-copy to copy the public part to the authorized_keys file on the server. This should be sufficient to get ssh up and running without passwords. A further step to perform for ssh hardening is to disable root access by applying changes to the ssh server config file ( /etc/sshd_config). This is performed as one of the final steps in the configure process.

```
ssh-keygen -t ed25519
...
...complete the key generation process...
```
Then update the group_vars/main.yml file with the path to wherever you placed the file.

# don't add ~/ as this will be detected later using the env 'HOME' variable.
ssh_key_dir:  .ssh
# name of the private key
sshk_key_name: ed25519 

## Further TODOs

- Add DNS setup so the server is allocated a proper name under my domain.

- Post install reporting to remind me to us e a specific SSH identity file.
