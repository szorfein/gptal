<p align="center">
<img src="https://github.com/szorfein/gptal/blob/main/.github/assets/logo-search-grid-2x.png?raw=true" height="661" weight="530" />
<!--
<img src="https://github.com/szorfein/gptal/blob/main/.github/assets/download (5).jpeg?raw=true" height="661" weight="530" />
<img src="https://github.com/szorfein/gptal/blob/main/.github/assets/download (4).jpeg?raw=true" height="661" weight="530" />
<br />
-->
</p>

<h1 align="center"></h1>
<div align="center" style="margin-top: 2rem;">
<p>
<a href="https://github.com/szorfein/gptal/stargazers"><img src="https://img.shields.io/github/stars/szorfein/gptal?style=for-the-badge&logo=starship&color=C9CBFF&logoColor=C9CBFF&labelColor=302D41" alt="stars"><a>&nbsp;&nbsp;
<a href="https://github.com/szorfein/gptal/"><img src="https://img.shields.io/github/repo-size/szorfein/gptal?style=for-the-badge&logo=hyprland&logoColor=f9e2af&label=Size&labelColor=302D41&color=f9e2af" alt="REPO SIZE"></a>&nbsp;&nbsp;
<a href="https://github.com/szorfein/gptal/commits/main/"><img src="https://img.shields.io/github/last-commit/szorfein/gptal?style=for-the-badge&logo=github&logoColor=eba0ac&label=Last%20Commit&labelColor=302D41&color=eba0ac" alt="Last Commit"></a>&nbsp;&nbsp;
<a href="https://github.com/szorfein/gptal/blob/main/LICENSE"><img src="https://img.shields.io/github/license/szorfein/gptal?style=for-the-badge&logo=&color=CBA6F7&logoColor=CBA6F7&labelColor=302D41" alt="LICENSE"></a>&nbsp;&nbsp;
</p>

Manage your secret GPG keys through portals.
</div>

chezmoi can bootstrap your entire environment. The problem is that when you use GPG to manage your "secrets", you need to import your private key "from the dark" before you can do anything. This also affect private Git repository, Duplicity, gpgtar, pass, gopass, and all tools that work with GPG. You can use rsync to restore your data (including keys, which need to be exported, a process that requires scripting) but this can take a "very" long time depending on the size of your backup.

Ellyx offers a simple solution to get your keys faster, plug an USB, type `ellyx --summon-lesser <recipient> --portal /dev/sdc`, enter a password and your keys are imported and ready to use.

## Features

- Private Key Backup and Recovery with Encryption
- Easy keys management allowing you to create subkeys with short expiring time (3month or less)
- Create "lesser keys" which can be used everyday (keys with `sec#`)
- Easy to use

## Development

- Alpha, (current stage)
- Beta
- Release

#### Encryption

I'll checking for a solution which not require "permission" and finally opt for [gocryptfs](https://nuetzlich.net/gocryptfs/), this tool has few advantages:

- use FUSE, which allow user to use `mount`, `unmount` without privilege
- ToMySelf: deal with "keep a copy of your master key in safe place?"

Other options:

- Luks, complicated to configure without [root permission](https://possiblelossofprecision.net/?p=300)
- gpgtar, also use gpg, will don't work
- EncFS, deprecated
- EcryptFS, deprecated

## Install

```
sudo xbps-install -S go gnupg gocryptfs
git clone https://github.com/szorfein/ellyx.git ~/.ellyx
cd ~/.ellyx && make install
```

The Makefile install `ellyx` in `$HOME/bin` so no extra permission are needed

## Usage

```sh
ellyx --summon-master <recipient> --portal /dev/k_666
ellyx --summon-lesser <recipient> --portal /dev/k_666
ellyx --summon-path ~/.gpg/my-key.zip
ellyx --expulse <recipient> --portal /dev/k_666
ellyx --dispell
```

## Mount (usb) portal without privilege

```sh
ellyx --help-udev --portal /dev/k_666
generated file >> /tmp/udev-rules
```

Save it as /etc/udev/rules.d/99-unlock-lucks.rules and reload all udev rules with

    # udevadm control --reload-rules
