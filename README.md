# Webarchitects Firefox Ansible Role

[![pipeline status](https://git.coop/webarch/firefox/badges/main/pipeline.svg)](https://git.coop/webarch/firefox/-/commits/main)

This is an Ansible role for installing Firefox on x86_64 Debian or Ubuntu (untested on other architectures or distros), it can be used with the [localhost playbook](https://git.coop/webarch/localhost).

See the [defaults](defaults/main.yml) for the different versions of Firefox that can be downloaded, installed and updated.

It is designed to be run using `sudo` or as `root` and works by:

1. Installing the [Mozilla Software Releases GPG key](https://blog.mozilla.org/security/2021/06/02/updating-gpg-key-for-signing-firefox-releases/).
2. Loading the [Firefox versions JSON](https://product-details.mozilla.org/1.0/firefox_versions.json).
3. Checking the locally installed version and if the latest available version is newer then:
   - Downloading Firefox and the GPG signature.
   - Verifying the downloaded files using the GPG signature.
   - Installing Firefox in `/opt`, adding a script to launch it in `/usr/local/bin` and creating a `.desktop` file in `/usr/share/applications`.

## Firejail

By default this role configures the Firejail for all the versions that are installed to be run via [Firejail](https://github.com/netblue30/firejail), which is installed from [Debian backports](https://backports.debian.org/) if available.

If you need to use WebAuthn (for example using a Yubikey) then you need to edit `/etc/firejail/firejail.config` to set:

```
browser-disable-u2f no
```

## Repo

The primary URL of this repo is [`https://git.coop/webarch/firefox`](https://git.coop/chriscroome/firefox) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-firefox) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/firefox).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/firefox/-/releases).
