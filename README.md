# Webarchitects Firefox Ansible Role

This is an Ansible role for installing Firefox on x86_64 Debian or Ubuntu
(untested on other architectures or distros), it can be used with the
[localhost playbook](https://git.coop/webarch/localhost).

See the [defaults](defaults/main.yml) for the different versions of Firefox
that can be downloaded, installed and updated.

It works by:

1. Installing the [Mozilla Software Releases GPG
   key](https://blog.mozilla.org/security/2021/06/02/updating-gpg-key-for-signing-firefox-releases/).
2. Loading the [Firefox versions
   JSON](https://product-details.mozilla.org/1.0/firefox_versions.json).
3. Checking the locally installed version and if the latest available version
   is newer downloads Firefox and the GPG signature, verifies it and installs
   it in `/usr/local` and symlinks it from `/usr/local/bin`.
