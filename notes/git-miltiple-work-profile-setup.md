How to configure multiple git profiles in a system ?
----
## 1. Create ssh keys for all accounts

```shell
cd ~/.ssh

## work account key
ssh-keygen -t rsa -C "my-work-addr@work-email.com" -f "work"

## personal account key
ssh-keygen -t rsa -C "my-addr@my-email.com" -f "me"
```

This will generate the files `work, work.pub, me, me.pub` in `~/.ssh` folder. 


## 2. Add the keys to ssh agent

```shell
ssh-add -K ~/.ssh/work
ssh-add -K ~/.ssh/me
```

## 3. Restart the ssh-agent once 

```shell
eval `ssh-agent -s` 
```

## 4. Add the keys to respective github profiles. 

Use `pbcopy < ~/.ssh/work.pub` to copy the work public key to clipboard for pasting in github. So same way for other keys.


## 5. Create host aliases in ssh config.
Create/edit the ssh config file i.e. `~/.ssh/config` , add the following section.

```
Host work
    Hostname github.com
    User work-gh-user
    IdentityFile ~/.ssh/work
    
Host me
    Hostname github.com
    User my-gh-user
    IdentityFile ~/.ssh/me
```
## 6. Clone using different profiles
```shell
## clone a work project
git clone git@work:org/somerepo.git

## clone a personal project
git clone git@me:me/myrepo.git
```
