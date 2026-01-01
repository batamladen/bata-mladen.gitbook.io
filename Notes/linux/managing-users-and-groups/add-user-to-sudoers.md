# Add user to sudoers

from a user that already has sudo privs write:

```
sudo vim /etc/sudoers.d/user_name
```

in the file type:

```
user_name ALL=(ALL) NOPASSWD: ALL
```

Change permisions

```
sudo chmod 440 /etc/sudoers.d/user_name
```

