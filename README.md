# Nginx Configuration
## Preface
[@carlbennett](https://github.com/carlbennett) wanted an nginx configuration
that was both secure and modular enough that he could put it on any server and
tune just a few small settings to make it work anywhere.

And thus, this configuration was born.

It is based off of the Fedora 21 x86\_64 nginx package and has been maintained
ever since. It is fully compatible with Fedora 24 x86\_64 and CentOS 7.x
x86\_64.

Recommended Nginx version: `1.8.0` or newer.

## Features
- Global caching
 - If included, tunes nginx to have browsers cache static resources.
- Global Gzip compression
 - If included, common types of static resources will be compressed by nginx.
- Global URL filtering
 - If included, nginx will disconnect common types of attacks based on the URL,
   instead of responding with an error page or content which could alert the
   bad actor about your server, which would send an invite to come back later.
 - Can be extended upon very easily to block even more types of URLs.
- PHP support
 - If included, you can _and should_ define php error reporting and short tags
   in your server block.
- New Relic support
 - You can _and should_ define the application name for New Relic in your
   server block.

## Installation
These steps have been tested on Fedora 24 x86\_64, and may require minor
changes to work on non-RHEL systems.

The following commands assume you are logged in as `root` or are `sudo`ing as
`root` before every command.

### Install nginx
```
dnf install nginx
```

\* If you are using CentOS, substitute `dnf` with `yum` in the command above.

### Setup the user and group
```
usermod -u 48 nginx
groupmod -g 48 nginx
groupadd -r www-data
usermod -aG www-data nginx
usermod -aG www-data `whoami`
```

### Clone this repository
```
cd ~
git clone git@github.com:carlbennett/nginx-conf.git
```

### Copy files to system
```
cp -r ./etc/nginx/ /etc/nginx
cp -r ./home/nginx/ /home/nginx
```

### File and directory permissions
```
chown root:root -R /etc/nginx
chown nginx:www-data -R /home/nginx
find /home/nginx -type f -print0 | sudo xargs -0 chmod 664
find /home/nginx -type d -print0 | sudo xargs -0 chmod 775
```

### SELinux permissions
```
restorecon -r /etc/nginx /home/nginx
```

### Configure nginx
You should now configure everything under `/etc/nginx` to your liking.

### Run nginx
```
systemctl enable nginx
systemctl start nginx
```
