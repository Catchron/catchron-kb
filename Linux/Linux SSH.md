## Secure Shell (SSH)
> `ssh` - The secure shell command. This allows us to make secure encrypted connections to remote systems

> `ssh-copy` - This command will copy our public ssh key to another system and set up the proper permissions for the `authorized_keys` file on the remote host

> `ssh-agent` - This command acts as a wrapper around an environment so that it can handle authentication for key files that use passphrases.

> `ssh-add` - This command adds the passphrase to the `ssh-agent`

The service that facilitates ssh connections is called the ssh daemon.

Its configuration file can be found at `/etc/ssh/sshd_config`

These are the locations for the HostKey files. These files determine what types of encrypted connections will be able to connect to our server.
- rsa
- dsa
- ecdsa
- ed25519

<img src="Images/5.png" width="300"/>

Currently they are located in `/etc/ssh` and are separated into public and private. Here also we can find the system files for the ssh service

<img src="Images/5.png" width="300"/>

Each user on the system that connects to other systems will also have files in their home directory under `/home/[user]/.ssh/`. These are the individual user configuration files. This folder will be empty or not existent if we havent made any connections from this user

<img src="Images/5.png" width="300"/>

To connect to a remote server we need to have its IP and the username we wish to connect as:
> `ssh [user]@[server-ip]`

When we connect to a remote system for the first time, we are shown the fingerprint of the keys from the ssh server on the remote computer.

These are the fingerprints from the same key that are located in `/etc/ssh` but on the remote system.

These fingerprints are added on our local system in the  `/home/[user]/.ssh/known_hosts` file.

The file contains the remote server IP, the encryption algorithm and the public key fingerprint from the remote system.

Why the `known_hosts` file is important is because if someone tempers with the ssh keys on the remote system, the next time we connect to it the ssh program will alert us that the key fingerprints are different and disallow the connection. This saves us from logging in to a potentially harmful host.

<img src="Images/5.png" width="300"/>

We can use key files so that we do not have to provide the remote account password and connect.

With these key files we are setting up a trust between the two systems where the keys are aware of each other and will let us authenticate using the key files instead of the user password.

In order to set this up we need to create a new public and private key files from our local user. You will need to store one of this file in to each remote user's local ssh directory.

> `ssh-keygen` - Generates a key-pair with a 2048 bit encryption by default using rsa encryption method.

> `ssh-keygen -b [encryption-bits] -t [encryption-method]` -  Generates a specifically chose by you encryption

The commands above will prompt you for the location where to store those pairs. In most cases this will be the user's ssh folder so just accept the default.

Second, the command will ask you to input a password for the key pair which will act as a multi-factor authentication.

<img src="Images/5.png" width="300"/>

The key pair is stored in the /home/[user].ssh/ folder.
- `id_rsa` - This is our private key. Do not share this key
- `id_rsa.pub` - This is our public key which needs to be placed on the remote system.

The public key will work with our private key in order to authenticate us. If either of these key files are changed we wont be able to authenticate.

<img src="Images/5.png" width="300"/>

The public key file's content is placed on the remote server in the user's `/home/[user]/.ssh/authorized_keys` file.

After this is done we can ssh directly to the remote server without providing the password for the remote user and only the password (if there is one) for the ssh key.

The `authorized_keys` file indicates the list of keys the system will accept connections from. All of those keys are the public keys from the systems that have been set to connect to it.

<img src="Images/5.png" width="300"/>
