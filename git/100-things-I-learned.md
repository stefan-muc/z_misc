# 100 Things I learned about git

1. One should do `git config --global core.autocrlf false` when working on Linux scripts on Windows git (and enable it manually for other repositories)
1. In Linux, git automatically respects the execute bit. In windows, you will have to add it manually using `git add --chmod=+x`
