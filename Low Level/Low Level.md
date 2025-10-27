[[System-level Programming]]
[[Why Some Projects Use Multiple Languages]]
[[Clang]]

## SSH security requirement
- file permissions are shown as three groups: **owner, group, others**.
	- **6** = read + write (4+2)
	- **0** = no permissions
So `600` means:
- **Owner (you)** → can read and write the file
- **Group** → no access
- **Others (everyone else)** → no access
```bash
-rw-------  
```

- private keys must **only** be accessible to you. If the permissions are too open (`644` or `666`), SSH refuses to use the key because it could be stolen by another user on the system

### Using `chmode 600` to change access mode for your private key
```bash
chmod 600 ~/.ssh/ssh_key
```
