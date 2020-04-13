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

See also the [geoipupdate role](https://git.coop/webarch/geoipupdate).

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
  <LocationMatch "/">
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
