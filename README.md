# trdesktop

Auto remote desktop to a Windows machines, all you need to do is provide a name
of it. This script use some options of rdesktop to make your session smoothly,
including `-b -t -D -P -r sound:off`, and a most important is resolution, it
use option `-g` with exactly your monitor resolution

This script use information from file named `list_machines.txt` in
/etc/trdesktop/ folder. Format of this file is:

- name: the name of machine you want to remote, e.g office
- address: ip address of it, e.g 192.168.1.100
- username: username in the machine you want to connect, e.g Administrator
- password: encoded password of username you want to connect, e.g xxxxxxxx
- domain: domain of the machine you want to connect, e.g HackMeBank

The main purpose of this script is encoded password, image that you use
rdesktop with option `-p my_secret_password` and somebody use command

```
ps aux | grep rdesktop
```

and they will see something like this

```
rdesktop 192.168.1.100 -u Administrator -p my_secret_password
```

Oh no, I can see your password because it is cleartext. Create an encoded
password by the below command

```
echo "my_secret_password" | base64
```

Put it to the file `list_machines.txt`, you will have something like this

```
office  192.168.1.100   Administrator   c2VjcmV0X3Bhc3N3b3Jk   'HackMeBank'
```

Now you can use this script to connect to the machine you called `office` in
the secure way, try to show process rdesktop on your computer one more time, it will show to you

```
rdesktop 192.168.1.100 -g 1366x768 -u Administrator -p XXXXXXXXX -d -b -t -D -P
-r sound:off
```

You can use IP address instead of the name, and of course, if no name or ip
address is exists in file `list_machines.txt`, this script would work fine,
except that you have to type username and password.

## trdesktop.patch

By default, you can not switch to another tag by hotkey when using rdesktop
because rdesktop get your hotkey. So I write this patch to bring it back to
you, just press hotkey twice and switch, I am very happy with it.

## Usage

Very simple to use

```
trdesktop office
```

Of course, every command you provide as arguments will be append to this
script. Example:

```
trdesktop office -r disk:share=/home/username/Downloads/
```

## Install

Script install in the folder will make a soft link from `trdesktop` into folder
/usr/bin. If you want ot delete this folder after `git clone`, you should cp
instead of ln

## Dependencies

rdesktop
xdotool

## License

All repository of me is under the GPLv3