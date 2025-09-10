# Outlook Tipps and Tricks

## Disable Shortcuts

### Alt-S

To disalble the [Alt]-[S] shortcut for sending emails, navigate to registry key `HKEY_CURRENT_USER\Software\Policies\Microsoft\office\16.0\outlook\DisabledShortcutKeysCheckBoxes` (you might have to create last key) and create a new `REG_SZ` Key with name `AltS` (can be choosen freely) and value `83,16` ([Source](https://robert365.com/article/disable-sending-via-ctrl-enter-or-alt-s))

## Ask for confirmation on [Ctrl]-[S]

This was a feature of Outlook classic that got removed. There's (currently) no way to get it back ([Source](https://learn.microsoft.com/en-us/answers/questions/4656221/ctrl-enter-combination-is-not-asking-a-confirmatio))