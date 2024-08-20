- Remote management tool to run commands on any other machine remotely.
- By default uses port 22

```
ssh username@<ip-address>
```

- CTRL+D to exit
- SSH keys can also be used to access the server without using the password.
- This is a more secure approach
- two keys are generated; a public key and a private key
- `ssh-keygen` is used to generate keys
- after generating keys the public key goes to the remote machine in the `.ssh` folder and the private key is kept locally.
- fallback is password by default if key authentication fails. This can be disabled.
- You can also add a passphrase while creating a keys which will be asked before using the key. This is kept locally and it offers more security. so the key cannot be used even if it is stolen
- `ssh-copy-id` can be used to copy id to the remote server
-  You can use the one key for multiple hosts
- 