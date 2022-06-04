## ssh-connect.md
SSH login

```bash
ssh ubuntu@13.215.251.129
```
face error ` (permission denied public key)

download `etcd.pem` key

change the ownership 

```bash
chown 400 etcd.pem
```
and use the command to enter the instance

```bash
ssh -i etcd.pem ubuntu@13.215.251.129
```
