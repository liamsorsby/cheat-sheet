# Generating a SSH key

### Generating an SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
If your system doesn't support ed25519 you can use the following:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

It's advised to use a password when using a SSH key.

To use the key you'll want to add it to your SSH config file by editing the following file `~/.ssh/config`
Add the following to your config changing the name if it's different.
```bash
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

To add the private key to your ssh-agent and your keychain run:
```bash
ssh-add -K ~/.ssh/id_ed25519
```

### Notes
If you're using ssh keys with git and it's asking for a password, make sure the config uses git@ and not https:// otherwise it will always ask for a password

### Navigation
[Back Home](../README.md)
