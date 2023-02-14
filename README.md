# minimalist-web-share

## Description

This project's goal is to share files using a "random" directory behind an HTTP server. It saves my Raspberry Pi 1 Model B+ from using TLS or a more sophisticated solution.

So:
* minimalist: a shell script to add files and a cron entry to remove them (if wanted);
* simple to use: run a script and send the resulting URL to who you want;
* single user: it fits my needs;
* secure administration: you know what you do;
* static HTTP configuration but dynamic content;
* idempotent according to filenames (and some stats).

## License

This project is distributed under the WTFPL 2 or later (see LICENSE for details).

## Installation

First, copy the files to your server:

```shell
me@local minimalist-web-share$ scp minimalist-web-share autoremove remote:
me@local minimalist-web-share$ ssh remote
me@remote ~$ sudo mv minimalist-web-share /usr/local/bin
me@remote ~$ sudo mv autoremove /etc/cron.daily/minimalist-web-share
```

set some environment variables:

```shell
me@remote ~$ MWS_WEB_ROOT=/var/www/share
me@remote ~$ MWS_BASE_URL="http://share.domain.tld"
```

and create the configuration file:

to define it globally:

```shell
me@remote ~$ sudo minimalist-web-share config "$MWS_WEB_ROOT" "$MWS_BASE_URL"
```

or only for the current user:

```shell
me@remote ~$ minimalist-web-share config "$MWS_WEB_ROOT" "$MWS_BASE_URL"
```

## Usage

After have uploaded your file somewhere on your remote host:

```shell
me@local ...$ ssh remote
me@remote ~$ sudo minimalist-webshare sharing-dir file1 file2
```

It prints the URL to sent or return an error.

## Sample with NGINX

```
server {
    listen 80;
    server_name share.hostname.tld;

    server_tokens off;
    charset UTF-8;

    access_log  /var/log/nginx/share_access.log;
    error_log   /var/log/nginx/share_error.log;

    location ~ ^(.+)$ {
        alias /var/www/share/$1;
        autoindex on;
    }
}
```
