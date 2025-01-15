---
description: (Host Based Enumeration)
---

# FTP

* on Linux most used FTP is vsFTPd
* there is also TFTP

***

## **TFTP**

| Command | Description                                                                                                                            |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| connect | Sets the remote host, and optionally the port, for file transfers.                                                                     |
| get     | Transfers a file or set of files from the remote host to the local host.                                                               |
| put     | Transfers a file or set of files from the local host onto the remote host.                                                             |
| quit    | Exits TFTP                                                                                                                             |
| status  | Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| verbose | Turns verbose mode, which displays additional information during file transfer, on or off.                                             |
| exit    | exit the ftp server                                                                                                                    |

***

## **vsFTPd**

#### install vsFTP

```bash
sudo apt install vsftpd
```

#### open config file

```bash
cat /etc/vsftpd.conf | grep -v "#"
```



| Setting                                                        | Description                                                                                              |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| listen=NO                                                      | Run from inetd or as a standalone daemon?                                                                |
| listen\_ipv6=YES                                               | Listen on IPv6 ?                                                                                         |
| anonymous\_enable=NO                                           | Enable Anonymous access?                                                                                 |
| local\_enable=YES                                              | Allow local users to login?                                                                              |
| dirmessage\_enable=YES                                         | Display active directory messages when users go into certain directories?                                |
| use\_localtime=YES                                             | Use local time?                                                                                          |
| xferlog\_enable=YES                                            | Activate logging of uploads/downloads?                                                                   |
| connect\_from\_port\_20=YES                                    | Connect from port 20?                                                                                    |
| secure\_chroot\_dir=/var/run/vsftpd/empty                      | Name of an empty directory                                                                               |
| pam\_service\_name=vsftpd                                      | This string is the name of the PAM service vsftpd will use.                                              |
| rsa\_cert\_file=/etc/ssl/certs/ssl-cert-snakeoil.pem           | The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |
| rsa\_private\_key\_file=/etc/ssl/private/ssl-cert-snakeoil.key |                                                                                                          |
| ssl\_enable=NO                                                 |                                                                                                          |



#### file for users that are denied from FTP service

```bash
cat /etc/ftpusers
```



### **Anonymous Settings**

<table><thead><tr><th width="294"></th><th></th></tr></thead><tbody><tr><td>anonymous_enable=YES</td><td>Allowing anonymous login?</td></tr><tr><td>anon_upload_enable=YES</td><td>Allowing anonymous to upload files?</td></tr><tr><td>anon_mkdir_write_enable=YES</td><td>Allowing anonymous to create new directories?</td></tr><tr><td>no_anon_password=YES</td><td>Do not ask anonymous for password?</td></tr><tr><td>anon_root=/home/username/ftp</td><td>Directory for anonymous.</td></tr><tr><td>write_enable=YES</td><td>Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE?</td></tr></tbody></table>



#### anonymous login

```bash
user: anonymous
pass: anonymous
```

#### see if anonymous login is allowed

```bash
nmap -sC {IP address}
```

{% hint style="warning" %}
**note:** at the ftp service it will say if its allowed, the default script scans the anonymous login.
{% endhint %}

***

## **More Info**

#### get the first overview of the server's settings

```sh
ftp> status
```

#### see more info

```sh
ftp> debug
ftp> trace
```



<table><thead><tr><th width="285">                Setting                  </th><th>Description</th></tr></thead><tbody><tr><td>dirmessage_enable=YES</td><td>Show a message when they first enter a new directory?</td></tr><tr><td>chown_uploads=YES</td><td>Change ownership of anonymously uploaded files?</td></tr><tr><td>chown_username=username</td><td>User who is given ownership of anonymously uploaded files.</td></tr><tr><td>local_enable=YES</td><td>Enable local users to login?</td></tr><tr><td>chroot_local_user=YES</td><td>Place local users into their home directory?</td></tr><tr><td>chroot_list_enable=YES</td><td>Use a list of local users that will be placed in their home directory?</td></tr></tbody></table>



|   |   |
| - | - |
|   |   |
|   |   |
|   |   |

