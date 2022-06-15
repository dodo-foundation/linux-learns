## WHAT IS SCP

SCP (Secure Copy Protocol) is a network protocol used to securely copy files/folders between Linux (Unix) systems on a network. 

SCP protects your data while copying across an SSH (Secure Shell) connection by encrypting the files and the passwords. 
Therefore, even if the traffic is intercepted, the information is still encrypted.


_**Use SCP when:**_

 * Copying files from a local host to a remote host.
 * Copying files from a remote host to a local host.
 * Copying files between two remote servers.


_Copy a File from a Remote Server to the Local Host_

* Push Method - File is transmitted from localhost to remote server 

```bash 

scp remote ip:/home/remote_dir/sample_example.txt home/Desktop

```

* Pull Method - File is receive from remote server to localhost

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

Push Method - File is transmitted from user to remote server 
 

```bash

scp username@userip:home/Desktop/sample_example.txt  root@remoteip:/home/remote_dir/

```

Pull Method - File is receive from remote server to user
 

```bash

scp root@remoteip:/home/remote_dir/sample_example.txt username@userip:home/Desktop/

```

If you Ignore Hostkey checking use `-o StrictHostKeyChecking=no`

_**SCP Command Options**_

* -C	Enable compression.
* -d	Copy the file, only if the destination directory already exists.
* -h	Show a list of command options.
* -r	Copy recursively.
* -u	Delete the source file once the copy is complete.
* -v	Enable verbose mode,
* -o ssh_option	Set options to SSH in ssh_config format.



