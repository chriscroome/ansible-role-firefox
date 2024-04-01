# Webarchitects Firefox Ansible Role

[![pipeline status](https://git.coop/webarch/firefox/badges/main/pipeline.svg)](https://git.coop/webarch/firefox/-/commits/main)

This is an Ansible role for configuring the Mozilla apt repo and installing all the Firefox editions.

## Usage

The suggested method for using this role is via the [localhost repo](https://git.coop/webarch/localhost).

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

### firefox

Set the `firefox` variable to `true` to run the tasks in this role, it defaults to `false`.

### firefox_editions

A list of Firefox editions to install, for example:

```yaml
firefox_editions:
 - name: Firefox Developer Edition
   app: firefox-devedition-latest
   pkg: firefox-devedition
 - name: Firefox Nightly
   app: firefox-nightly-latest
   pkg: firefox-nightly
```

By default the Firefox ESR version is not installed since this is provided by Debian.

### firefox_firejail

A boolean, install and configure Firejail, `firefox_firejail` defaults to `true`.

### firefox_lang

The Firefox language, `firefox_lang` defaults to `en-GB`.

### firefox_validate

Validate all variables that start with `firefox_` using the argument specification.

## Firejail

By default this role configures the Firejail for all the versions that are installed to be run via [Firejail](https://github.com/netblue30/firejail), which is installed from [Debian backports](https://backports.debian.org/) if available.

In order for WebAuthn to work (for example using a Yubikey) this role edits `/etc/firejail/firejail.config` to set:

```ini
browser-disable-u2f no
```

## Profiles

You can use the [Firefox Profile Manager](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) to manage and switch which version uses which profile, type `about:profiles` in the address bar to access the Profile Manager.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/firefox`](https://git.coop/chriscroome/firefox) however it is also [mirrored to GitHub](https://github.com/chriscroome/ansible-role-firefox) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/firefox).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/firefox/-/releases).

## Copyright

Copyright 2022-2024 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
