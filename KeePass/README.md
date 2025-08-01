# KeePass

KeePass is a free, open source, light-weight and easy-to-use password manager.

It can be found at <https://keepass.info/> and also on Chocolatey <https://community.chocolatey.org/packages/keepass>

## Default settings for security and convenience

Adjust this settings in *Options* dialogue:

* Security
    * [x] Lock workspace after global user inactivity (seconds): Insert reasonable time
    * General -> [x] Lock workspace when the computer is about to be suspended
    * General -> [x] Lock workspace when the remote control mode changes
* Interface
    * Entry List -> [x] Automatically resize entry list columns when resizing the main window
* Integration
    * [x] Run KeePass at Windows startup (current user)
* Advanced
    * Start and Exit -> [x] Automatically save when closing/locking the database
    * Start and Exit -> [x] Automatically save after modifying an entry using the entry editing dialog
    * Auto-Type - Sending -> [x] Cancel auto-type when the target window changes

## Auto-Sync database to a remote/synced database on opening

Remote/Synced databases shouldn't be accessed directly using KeePass but only used for syncing a local database to it.

So to use this auto sync trigger, do the following:

* Copy the `\\remote\database.kdbx` to a `D:\local\database.kdbx`.
* Copy trigger xml from below to a editor of your choice
* Replace example paths with the path you actually use. (escape XML special characters, e.g. use `&amp;` instead of `&`)
* Open *<u>T</u>ools -> T<u>r</u>iggers* and use *[<u>T</u>ools] -> <u>P</u>aste Triggers from Clipboard*.
* A new Trigger ``Sync password database ...` will appear
* Click `[OK]` to save the trigger
* Open your local database and see in status bar that the trigger works and databases are synced

```xml
<?xml version="1.0" encoding="utf-8"?>
<TriggerCollection xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<Triggers>
		<Trigger>
			<Guid>KfDie00o30uZSx08hTKr8Q==</Guid>
			<Name>Sync password database to a remote/synced database</Name>
			<Events>
				<Event>
					<TypeGuid>s6j9/ngTSmqcXdW6hDqbjg==</TypeGuid>
					<Parameters>
						<Parameter>0</Parameter>
						<Parameter>D:\local\database.kdbx</Parameter>
					</Parameters>
				</Event>
				<Event>
					<TypeGuid>5f8TBoW4QYm5BvaeKztApw==</TypeGuid>
					<Parameters>
						<Parameter>0</Parameter>
						<Parameter>D:\local\database.kdbx</Parameter>
					</Parameters>
				</Event>
			</Events>
			<Conditions>
				<Condition>
					<TypeGuid>y0qeNFaMTJWtZ00coQQZvA==</TypeGuid>
					<Parameters>
						<Parameter>\\remote\database.kdbx</Parameter>
					</Parameters>
					<Negate>false</Negate>
				</Condition>
			</Conditions>
			<Actions>
				<Action>
					<TypeGuid>tkamn96US7mbrjykfswQ6g==</TypeGuid>
					<Parameters>
						<Parameter />
						<Parameter>0</Parameter>
					</Parameters>
				</Action>
				<Action>
					<TypeGuid>Iq135Bd4Tu2ZtFcdArOtTQ==</TypeGuid>
					<Parameters>
						<Parameter>\\remote\database.kdbx</Parameter>
						<Parameter />
						<Parameter />
					</Parameters>
				</Action>
				<Action>
					<TypeGuid>tkamn96US7mbrjykfswQ6g==</TypeGuid>
					<Parameters>
						<Parameter />
						<Parameter>1</Parameter>
					</Parameters>
				</Action>
			</Actions>
		</Trigger>
	</Triggers>
</TriggerCollection>
```
