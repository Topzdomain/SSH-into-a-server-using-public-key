<h2 align="center"> SSH LOGIN INTO SERVER USING PUBLIC KEY</h2>

This project is to demonstrate my skill in SSH Technology to enable users log into a server using a public key rather than the use of password which is susceptible to brute forcing. This ssh login method uses Asymmetric Keys cryptographic technology which involves generating two sets of keys, a private and a public. The local host machine stores the private key securely while the server stores the public and both are matched at the point login, to grant access into the server. This project was created for access into an ubuntu server in AWS. Also, I have redacted the ip addresses in the screenshot for security purposes. 

- First is to log into the server using a default username and public key previously created.

```commandline
ssh ubuntu@server_ip_address
```
- Next, I created a new user on the server named Tony2. The name was not a regular expression based on server rules, so I had to force it.

```commandline
sudo adduser --force-badname
```
<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/user-creation-on-ubuntu.png" height="50%" width="60%"/>
</p>

- Switched into the new user account (Tony2) using su. On the new user account, I created a new directory .ssh and authorized_keys directory (the authorized_keys is supposed be a file and not a directory so I corrected it later in the project) inside the .ssh directory

<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/.ssh-and-authorized-keys-directory.png" height="50%" width="60%"/>
</p>

- Next, I changed permission for the directories. For .ssh directory only the file owner (u) has read, write and execute permission while groups and others do. For the authorized_keys directory, only the file owner (u) has read and write permissions, group and other users do not.
```commandline
chmod u=rwx,go= ~/.ssh/
chmod u=rw,go= ~/.ssh/authorized_keys
```
- On my Windows machine I generated the SSH keys. The .ssh directory already has SSH keys I generated previously so I created a new directory “New Key” to store the SSH keys and provided the directory path while creating the SSH keys.

<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/ssh-key-generation.png" height="50%" width="60%"/>
</p>

- The next step is to copy the public key from the Windows directory to the authorized_keys directory on the Ubuntu server. I was unable to do the copy directly from the Windows commandline (using ssh copy or scp protocol) as it was constantly throwing errors. So the workaround was for me to read the id_rsa.pub key using the type command. Manually copy the pub key, and pasted it on the Ubuntu server. Using Nano to edit the authorized_keys file, I pasted the copied key and saved it to the file.

<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/rsa-pub-key.png" height="70%" width="80%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/public-key-copied-to-authorized-keys-file.png" height="70%" width="80%"/>
</p>

- In the process of creating .ssh and authorized_keys for the new user Tony2 on Ubuntu, I made the mistake of creating the authorized_keys as a directory instead of a file and as a result, when I tried pasting the public key in the file, It didn’t work, the server flagged it as a directory. So I deleted the directory and re-created it as a file using the touch command, gave the appropriate permission, then pasted public key in it.

```commandline
touch /home/Tony2/.ssh/authorized_keys
```
<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/re-creating-authorized-keys-file.png" height="50%" width="60%"/>
</p>

- On my host machine in a new window terminal, I tried logging into the Ubuntu server using the new username I created and the public key and I was able to log in.

```commandline
ssh -i id_rsa Tony2@server_ip_address
```
<p align="center">
<img src="https://github.com/Topzdomain/SSH-into-a-server-using-public-key/blob/main/screenshots/access-gained-into-ubuntu-using-ssh-and-public-key.png" height="50%" width="60%"/>
</p>

