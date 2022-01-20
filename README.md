# Webarchitects Firefox Ansible Role

This is an Ansible role for installing Firefox on x86_64 Debian or Ubuntu
(untested on other architectures), it can be used with the [localhost
playbook](https://git.coop/webarch/localhost).

See the [defaults](defaults/main.yml) for the different versions of Firefox
that can be downloaded, installed and updated.

The way it works is it:

1. Installs the [Mozilla Software Releases GPG
   key](https://blog.mozilla.org/security/2021/06/02/updating-gpg-key-for-signing-firefox-releases/).
2. Loadis the [Firefox versions
   JSON](https://product-details.mozilla.org/1.0/firefox_versions.json).
3. Checks the locally installed versions and if the latest version is newer
   downloadeck the GPG signature and install it in `/usr/local` and symlink it
   from `/usr/local/bin`.
