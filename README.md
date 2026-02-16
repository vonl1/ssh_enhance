

### Tested system information

- System: Rocky 9.7
- Firewall application: firewalld

### Use example

```
---
- name: ssh_enhance
  host: your_target
  roles:
    - ssh_enhance
  # the default ssh_port is 65535
  vars:
    ssh_port: your_specify_port
```

Attenstion!  This role will close selinux.

SSH public key is on the ssh_enhance/files location, you need to mv your public key to ssh_enhance/files.

If your public key name is not `id_ed25519.pub` and `id_ed25519_sk.pub`, please change the 28 line of `tasks/main.yml` 

### Task info

1. Disable SELinux
2. Open the {{ssh_port}}/tcp port in firewalld
3. Upload the public key from `roles/ssh_enhance/files`
4. Fix the GSSAPI error in `50-redhat.conf`
5. Enable post-quantum (PQ) key exchange algorithms
6. Force enable Ed25519 algorithm support
7. Configure SSH to listen on port {{ssh_port}}
8. Configure SSH to disable password login for the root user
9. Disable password authentication
10. Set the maximum number of authentication attempts to 3
11. Remove the SSH (22) port from firewalld

### Config SSH key 

#### Gen SSH key

```
# gen anti quantum ssh key
ssh-keygen -t ed25519-sk -O verify-required -C "username@example.com"

# gen anti brute force crack ssh key
ssh-keygen -t ed25519 -a 100 -C "username@example.com"
```

Attention!  ed25519-sk type keys require an external entity security key, such as yubikey.

#### MV public key to ssh_enhance/files


