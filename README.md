# Webarchitects MaxMind DB Apache Module Ansible role

An Ansible role to download, compile and install the [latest version](https://github.com/maxmind/mod_maxminddb/releases) of [MaxMind DB Apache Module](https://github.com/maxmind/mod_maxminddb) on Debian and Ubuntu.

See also the [geoipupdate role](https://git.coop/webarch/geoipupdate).

## Role variables

Set `modmaxminddb` to `true` for the tasks in this role to be run.

## Examples

Front page based on country:

```apache
# {{ ansible_managed }}
<IfModule maxminddb_module>
  # https://github.com/maxmind/mod_maxminddb#directives
  MaxMindDBEnable On
  MaxMindDBFile COUNTRY_DB /usr/share/GeoIP/GeoLite2-Country.mmdb
  MaxMindDBEnv MM_COUNTRY_CODE COUNTRY_DB/country/iso_code
  # UK Homepage
  SetEnvIf MM_COUNTRY_CODE ^(GB|IM|JE|GG) MM_REDIRECT=uk
  # North America Homepage
  SetEnvIf MM_COUNTRY_CODE ^(US|MX|CA) MM_REDIRECT=na
  <LocationMatch "^/$">
    <If "-z %{ENV:MM_REDIRECT}">
      # Global Homepage
      Redirect "https://%{HTTP_HOST}/global/"
    </If>
    <Else>
      Redirect "https://%{HTTP_HOST}/%{ENV:MM_REDIRECT}/"
    </Else>
  </LocationMatch>
</IfModule>
# vim: set ft=apache:
```

## Notes

This Apache module isn't available from Debian but there is a (2018) [pull request regarding Debian packaging](https://github.com/maxmind/mod_maxminddb/pull/58).

Note that this uses the [Debian version of libmaxminddb](https://packages.debian.org/buster/libmaxminddb0) which is currently 1.3, rather than the [latest version](https://github.com/maxmind/libmaxminddb/releases/latest), however if [this issue is resolved](https://github.com/maxmind/libmaxminddb/issues/225) then perhaps Debian will package a newer version.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/modmaxminddb`](https://git.coop/webarch/modmaxminddb) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-modmaxminddb) and [available via Ansible
Galaxy](https://galaxy.ansible.com/chriscroome/modmaxminddb).

See the [GitLab releases page](https://git.coop/webarch/modmaxminddb/-/releases) for details regarding each version, *please use a specific version* since the master branch is used for development.

## Copyright

Copyright 2020-2025 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
