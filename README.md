Ansible role to download, compile and install the 
[latest version](https://github.com/maxmind/mod_maxminddb/releases) of 
[MaxMind DB Apache Module](https://github.com/maxmind/mod_maxminddb).

This Apache module isn't available from Debian but there is a (2018) 
[pull request regarding Debian packaging](https://github.com/maxmind/mod_maxminddb/pull/58).

Note that this uses the 
[Debian version of libmaxminddb](https://packages.debian.org/buster/libmaxminddb0) 
which is currently 1.3, rather than the 
[latest version](https://github.com/maxmind/libmaxminddb/releases/latest),
however if [this issue is resolved](https://github.com/maxmind/libmaxminddb/issues/225) 
then perhaps Debian will package a newer version.

