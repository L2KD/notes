## Audit

```
firejail --audit firefox
```

## Local config

```bash
$ touch ~/.config/firejail/firefox-developer-edition.local
```

```
# ~/.config/firejail/firefox-developer-edition.local

# Allow dbus (input methods)
ignore nodbus

# Allow yubikey to work
ignore nou2f
```
