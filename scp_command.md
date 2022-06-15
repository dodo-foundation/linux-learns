## WHAT IS SCP

SCP (Secure Copy Protocol) is a network protocol used to securely copy files/folders between Linux (Unix) systems on a network. 

SCP protects your data while copying across an SSH (Secure Shell) connection by encrypting the files and the passwords. 
Therefore, even if the traffic is intercepted, the information is still encrypted.

_**Use SCP when:**_

 * Copying files from a local host to a remote host.
 * Copying files from a remote host to a local host.
 * Copying files between two remote servers.


_Copy a File from a Remote Server to the Local Host_

Push Method --> file sent to localhost to remote server 

```bash 

scp remote ip:/home/remote_dir/sample_example.txt home/Desktop

```

Pull Method --> file receive from remote server to localhost

```bash

scp home/Desktop/sample_example.txt remote ip:/home/remote_dir/

```
In this case ill transfer a **backup** folder in localhost to server

```bash

scp -r home/Desktop/backup remote ip:/home/remote_dir/

```

for More scp command option given to below

---

_Copy a File from One Remote Server to Another_

Push Method --> file sent to user side to remote server 

```bash

scp username@userip:home/Desktop/sample_example.txt  root@remoteip:/home/remote_dir/

```

Push Method --> file receive from remote server to user 

```bash

scp root@remoteip:/home/remote_dir/sample_example.txt username@userip:home/Desktop/

```

_**SCP Command Options**_




