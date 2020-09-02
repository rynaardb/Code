# SSH

```bash
# Generate a new public & private key pair
ssh-keygen

# Quickly copy SSH public key
pbcopy < ~/.ssh/id_rsa.pub

# Generate a public key from your private key
ssh-keygen -y -f path_to_private_key

# Copy SSH public key to remote host (password-less login)
ssh-copy-id user@remotehost

# Log in using private key
ssh -i path_to_private_key user@remotehost
```
