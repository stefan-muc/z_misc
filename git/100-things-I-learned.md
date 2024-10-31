# 100 Things I learned about git

1. One should do `git config --global core.autocrlf false` when working on Linux scripts on Windows git (and enable it manually for other repositories)
1. In Linux, git automatically respects the execute bit. In windows, you will have to add it manually using `git add --chmod=+x`
1. While a `git blame` shows you the last commit that changed a line, `git log -G"<regex>"` will show you all commits that touched the line matching the regex
1. Commiting with `--author` doesn't need to specifiy full author and mail address - just use a part of it and git will reverse-search last commits for full string
