# OneDrive Tipps and Tricks

## Download all files

Sometimes the `Download all files` button in OneDrive is greyed out. To manually download all files once use this in root directory in a bash:

```bash
find -name "*" -type f -exec head -n 1 > /dev/null {} \;
```
