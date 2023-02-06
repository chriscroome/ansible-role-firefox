# Webarchitects Firefox Ansible Role

[![pipeline status](https://git.coop/webarch/firefox/badges/main/pipeline.svg)](https://git.coop/webarch/firefox/-/commits/main)

This is an Ansible role for installing and updating multiple Firefox versions on x86_64 and i386 Debian and Ubuntu GNU/Linux.

## Usage

This role is designed to be run using `sudo` or as `root` and works by:

1. Installing the [Mozilla Software Releases GPG key](https://blog.mozilla.org/security/2021/06/02/updating-gpg-key-for-signing-firefox-releases/).
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

# Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

### firefox

Set the `firefox` variable to `true` to run the tasks in this role, it defaults to `false`.

### firefox_bin

The `bin` directory to use, `firefox_bin` defaults to `/usr/local/bin`.

### firefox_download

The temporary directory to use for downloading the Firefox archive, `firefox_download` defaults to `/tmp`.

### firefox_editions

A list of Firefox versions to install, for example for the latest stable release:

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

By default the Firecfor ESR version is not installed since this is provided by Debian.

The `firefox_editions` list variables are:

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

## Firejail

By default this role configures the Firejail for all the versions that are installed to be run via [Firejail](https://github.com/netblue30/firejail), which is installed from [Debian backports](https://backports.debian.org/) if available.

If you need to use WebAuthn (for example using a Yubikey) then you need to edit `/etc/firejail/firejail.config` to set:

```
browser-disable-u2f no
```

## Repository

The primary URL of this repo is [`https://git.coop/webarch/firefox`](https://git.coop/chriscroome/firefox) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-firefox) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/firefox).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/firefox/-/releases).

## Copyright

Copyright 2022-2023 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
