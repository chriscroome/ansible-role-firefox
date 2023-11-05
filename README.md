# Webarchitects Firefox Ansible Role

[![pipeline status](https://git.coop/webarch/firefox/badges/main/pipeline.svg)](https://git.coop/webarch/firefox/-/commits/main)

This is an Ansible role for installing and updating multiple Firefox versions on x86_64 and i386 Debian and Ubuntu GNU/Linux, by default the [Mozilla apt repo is used for Firefox Nightly](https://blog.nightly.mozilla.org/2023/10/30/introducing-mozillas-firefox-nightly-deb-packages-for-debian-based-linux-distributions/) and binaries are downloaded and installed for the latest, beat and developer edition.

## Usage

This role configures the [Mozilla’s Firefox Nightly apt repo](https://blog.nightly.mozilla.org/2023/10/30/introducing-mozillas-firefox-nightly-deb-packages-for-debian-based-linux-distributions/) and for other Firefox versions it works by:

1. Installing the [Mozilla Software Releases GPG key](https://blog.mozilla.org/security/2023/05/11/updated-gpg-key-for-signing-firefox-releases/).
2. Loading the [Firefox versions JSON](https://product-details.mozilla.org/1.0/firefox_versions.json).
3. Checking the locally installed version and if the latest available version is newer then:
   - Downloading Firefox and the GPG signature.
   - Verifying the downloaded files using the GPG signature.
   - Installing Firefox in `/opt`, adding a script to launch it in `/usr/local/bin` and creating a `.desktop` file in `/usr/share/applications`.

The suggested method for using this role is via the [localhost repo](https://git.coop/webarch/localhost) which contains a [firefox.sh](https://git.coop/webarch/localhost/-/blob/main/firefox.sh) script that will download this role and run it, for example:

```bash
git clone https://git.coop/webarch/localhost.git
cd localhost
./firefox.sh
```

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

### firefox

Set the `firefox` variable to `true` to run the tasks in this role, it defaults to `false`.

### firefox_bin

The `bin` directory to use, `firefox_bin` defaults to `/usr/local/bin`.

### firefox_download

The temporary directory to use for downloading the Firefox archive, `firefox_download` defaults to `/tmp`.

### firefox_editions

A list of Firefox binary versions to install, for example for the latest stable release:

```yaml
firefox_editions:
  - name: Firefox
    channel: desktop_release
    path: firefox-latest
    product: firefox-latest
    script: firefox
    version: LATEST_FIREFOX_VERSION
    jail: true
```

By default the Firefox ESR version is not installed since this is provided by Debian and Firefox Nightly is also not installed since it is provided by the apt repo.

The `firefox_editions` list item variables are:

#### name

The Firefox version name from the [select list](https://www.mozilla.org/en-GB/firefox/all/), the choices are:

```yaml
- Firefox
- Firefox Beta
- Firefox Developer Edition
- Firefox Nightly
- Firefox Extended Support Release
```

#### channel

The Firefox channel from the [select list](https://www.mozilla.org/en-GB/firefox/all/), the choices are:

```yaml
- desktop_release
- desktop_beta
- desktop_developer
- desktop_nightly
- desktop_esr
```

#### path

The path name that firejail uses, the options are:

```yaml
- firefox-latest
- firefox-beta
- firefox-developer-edition
- firefox-nightly
- firefox-esr
```

#### product

The Firefox version from the [download URL](https://www.mozilla.org/en-GB/firefox/all/), the choices are:

```yaml
- firefox-latest
- firefox-beta-latest
- firefox-devedition-latest
- firefox-nightly-latest
- firefox-esr-latest
```

#### script

The name of the script used to run Firefox.

#### version

Firefox version taken from the [Firefox JSON versions file](https://product-details.mozilla.org/1.0/firefox_versions.json), the options are:

```yaml
- LATEST_FIREFOX_VERSION
- LATEST_FIREFOX_DEVEL_VERSION
- FIREFOX_DEVEDITION
- FIREFOX_NIGHTLY
- FIREFOX_ESR
```

#### jail

Run Firefox in a firejail, `jail` is a boolean, it defaults to `true` for all versions.

### firefox_firejail

A boolean, install and configure Firejail, `firefox_firejail` defaults to `true`.

### firefox_lang

The Firefox language, `firefox_lang` defaults to `en-GB`.

### firefox_nightly_apt

A boolean, install Firefox Nightly using `apt`, `firefox_nightly_apt` defaults to `true`.

### firefox_nightly_apt_firejail

Firejail the `apt` version of Firefox Nightly, `firefox_nightly_apt_firejail` defaults to `true`.

### firefox_path

The directory in which the Firefox binary versions should be installed, `firefox_path` defaults to `/opt`.

### firefox_validate

Validate all variables that start with `firefox_` using the argument specification.

## Firejail

By default this role configures the Firejail for all the versions that are installed to be run via [Firejail](https://github.com/netblue30/firejail), which is installed from [Debian backports](https://backports.debian.org/) if available.

In order for WebAuthn to work (for example using a Yubikey) this role edits `/etc/firejail/firejail.config` to set:

```ini
browser-disable-u2f no
```

## Profiles

You can use the [Firefox Profile Manager](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) to manage and switch which version uses which profile.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/firefox`](https://git.coop/chriscroome/firefox) however it is also [mirrored to GitHub](https://github.com/chriscroome/ansible-role-firefox) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/firefox).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/firefox/-/releases).

## Copyright

Copyright 2022-2023 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
