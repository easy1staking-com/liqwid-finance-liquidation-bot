# Disable password auth for SSH

Many of you will recur to use a Virtual Private Server (VPS) to run their BOTs.

Usually logging onto such VPS is done via `ssh` by specifying `username` and `password`. This is of course mandatory
when you're logging onto your servers for the first time, but afterwords this is discouraged as it represents a 
vulnerability.

Security needs to be taken seriously when setting up software that manages your funds. Specially when it runs on remote
hosts that are accessible from the internet.

Today we're going to discuss a simple but efficient configuration update to the SSH daemon that will increase the security of your VPS.

The SSH daemon by default allows multiple way of authenticating. By default, the password method is enabled because it's the
easiest to get started with, **but** if security level must be increased, it then needs to be disabled in favour of a
more secure approach.

In this paragraph we will learn how to replace, once again un ubuntu linux, the username+password `ssh` authentication 
method with [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) keys 

## Generating new keys

The first step is to create a new set of keys that will then be used to _ssh_ onto your VPS.

> **NOTE** you may already have some keys created for some other project, these are valid and can definitely be
> used for this case. Usually the folder in which these keys are created is ~/.ssh, so if you are in doubt, just issue
> `ls -lart ~/.ssh`. If there is nothing, follow instructions below on how to create new ones. If you already have some
> feel free to use them and skip the next section. If you have some, but have no idea what they are and what you used them
> for already, you may consider creating new ones.

In order to create a new set of keys issue:

```ssh-keygen```

It will prompt for some questions like:
* where to create them
* what password you want to secure these keys with

For the purposes of this tutorial I'm just going passwordless and use default folder, but you should definitely set a password
to increase even more the security of your keys.

You'll be prompted with something like this:

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/gio/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/gio/.ssh/id_rsa.
Your public key has been saved in /Users/gio/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:n8n8b8mt5AWhxJ4UF2O3qj+Jzf7wFZER1gDsAOwkg4k gio@Giovannis-MacBook-Pro.localdomain
The key's randomart image is:
+---[RSA 3072]----+
|   . o ... .o.**+|
|  E o o o ...+.o=|
|       =   o+ .+ |
|        .  +.o...|
|        S   +... |
|         + o.  ..|
|          *.+oooo|
|           o.B*.o|
|            o=*+ |
+----[SHA256]-----+
```

Great, you got your keys created. Now remember to save a copy somewhere safe. If you loose them, you won't be able to
_ssh_ on your VPS anymore.

## Disable SSH w/ password

These steps are taken from [source](https://www.cyberciti.biz/faq/how-to-disable-ssh-password-login-on-linux/).

The first step is to "install" your key on the VPS list of allowed keys.

`ssh-copy-id -i id_rsa.pub ubuntu@1.2.3.4`

Where the `1.2.3.4` is the ip of your VPS.

You'll be asked for the usual password. Hopefully, the last time you'll ever enter it.

Ssh on the VPS and update SSH server configuration with:

`sudo vi /etc/ssh/sshd_config`

Set following values

```
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
PermitRootLogin no
```

Restart ssh server

`sudo service ssh reload`

And you're done. No more password entering, this means that, if someone gets on your network won't be able to
brute force username/password as it's disabled.

### How to test

ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no <host> -l <user>

[source](https://unix.stackexchange.com/questions/15138/how-to-force-ssh-client-to-use-only-password-auth)
