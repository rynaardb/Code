# macOS

## Keyboard

```bash
# Set initial key repeat
defaults write -g InitialKeyRepeat -int 10

# Set key repeat
defaults write -g KeyRepeat -int 2
```

## Hostname

```bash
sudo scutil --set HostName <computername>
sudo scutil --set LocalHostName <computername>
sudo scutil --set ComputerName <computername>
```