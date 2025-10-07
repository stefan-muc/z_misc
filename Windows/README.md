# Microsoft Windows

## Recycle bin on subst'ed drives

Execute this in PowerShell to add a recycle bin for subst'ed drive `D:`

```powershell
$guid=$("{"+[guid]::NewGUID().ToString().ToUpper()+"}")
$key="HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\$guid"
reg add "$key" /v RelativePath /t REG_SZ /d D:\
reg add "$key" /v Name /t REG_SZ /d DriveD
reg add "$key" /v Category /t REG_DWORD /d 4
```

Source: [superuser.com](https://superuser.com/a/1902811)