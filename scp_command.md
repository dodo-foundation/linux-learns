## WHAT IS SCP

SCP (Secure Copy Protocol) is a network protocol used to securely copy files/folders between Linux (Unix) systems on a network. 

SCP protects your data while copying across an SSH (Secure Shell) connection by encrypting the files and the passwords. 
Therefore, even if the traffic is intercepted, the information is still encrypted.

_**Use SCP when:**_

 * Copying files from a local host to a remote host.
 * Copying files from a remote host to a local host.
 * Copying files between two remote servers.


_Copy a File from a Remote Server to the Local Host_

Pull Method --> file sent to remote server to localhost 

```bash 

scp remote ip:/home/remote_dir/sample_example.txt home/Desktop

```

Push Method --> file receive from localhost to remote server

```bash

scp home/Desktop/sample_example.txt remote ip:/home/remote_dir/

```

_Copy a File from One Remote Server to Another_

Pull Method --> file sent to root server to user

```bash

scp root@remoteip:/home/remote_dir/sample_example.txt username@userip:home/Desktop

```

Push Method --> file sent to root server to user

```bash

scp username@userip:home/Desktop/sample_example.txt root@remoteip:/home/remote_dir/

```
