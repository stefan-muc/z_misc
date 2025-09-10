# yadm - Yet Another Dotfiles Manager

See [Yet Another Dotfiles Manager Website](https://yadm.io/) for project information.

## Installation in Windows

Clone git repository to a location of your choice and check out latest version in a bash shell:

```bash
cd /d/public/github.com/yadm-dev
git clone https://github.com/yadm-dev/yadm 
cd yadm
git checkout `git tag -l --sort=-creatordate | head -n 1`
```

Link yadm to your executables in a CMD shell:

```cmd
mklink /d %USERPROFILE%\bin\yadm d:\public\github.com\yadm-dev\yadm\yadm
```

(create a hard link using `/h` instead of a symbolic `/d` if you are not an admin user)

Test in bash shell if yadm is ready to rumble:

```bash
yadm --version
```
