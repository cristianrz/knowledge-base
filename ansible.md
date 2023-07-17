# Ansible

## Ad-hoc command

```bash
ansible \
	--become \
	--ask-become-pass \
	--verbose \
	--inventory inventory.yaml \
	--args "echo hello world" \
	servers
```
