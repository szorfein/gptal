<p align="center">
<img src="https://github.com/szorfein/gptal/blob/main/.github/assets/gptal.png?raw=true" height="661" weight="530" />
<br />
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

chezmoi can bootstrap your whole environment, the problem is when you use GPG to manage your "secret", you need to import your private key "from the dark" before making anything. This concern also github (private repository), duplicity, gpgtar, pass, gopass, and all tools which work with GPG. You can use rsync to restore your data (keys include, they need to be exported which require scripting only for this...) but it can be "very" long depending of your backup size.

Ellyx offer a simple solution to get your keys faster, plug an USB, type `ellyx --summon-lesser -k <recipient> --portal /dev/sdc`, type a password and your keys are imported and ready to use.

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

## Usage

```sh
ellyx --summon-master --key <recipient> --portal /dev/k_666
ellyx --summon-lesser --key <recipient> --portal /dev/k_666
ellyx --summon-path ~/.gpg/my-key.zip
ellyx --expulse --key <recipient> --portal /dev/k_666
ellyx --dispell
```

## Mount portal without privilege

```sh
ellyx --help-udev --portal /dev/k_666
generated file >> /tmp/udev-rules
```

Save it as /etc/udev/rules.d/99-unlock-lucks.rules and reload all udev rules with

    # udevadm control --reload-rules
