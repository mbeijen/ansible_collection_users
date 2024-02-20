# Ansible Collection l3d.users
Ansible Collection to manage Users, Groups and SSH Keys.

There are multiple ansible roles in this collection. Togehter they can setup an unix system with proper users, groups and if they need it supoerpowers. The user could get SSH Keys or a Password. It is also possible to limit the login via SSH to the defined users.
And it is possible to delete users too.

 Ansible Roles:
-----------------
*Please note, it is pretty useless to add an ssh key to an non-existing user directory. So please add users first before running other roles*
+ ``l3d.users.user``: [roles/user](roles/user)
+ ``l3d.users.admin``: [roles/admin](roles/admin)
+ ``l3d.users.sshd``: [roles/sshd](roles/admin)

 Global Variables:
-------------------

### User Management

+ The dictionary-variable for your group_vars to set your general users and admins is ``l3d_users__default_users``.
+ The dictionary-variable for your host_vars to set your host-specific users and admins is: ``l3d_users__local_users``.
The Option of these directory-variables are the following.

| option | values | required | description |
| ------ | ------ | --- | --- |
| ``name``   | *string* | ``required`` | The user you want to create |
| ``state``  | ``present`` | - | Create or delete user |
| ``shell`` | ``/bin/bash`` | - | The Shell of the User |
| ``create_home`` | ``true`` | - | create a user home *(needed to store ssh keys)* |
| ``admin`` | ``false`` | - | enable it to give the user superpowers |
| ``admin_commands`` | *string or list* | - | Commands that are allows to be run as admin, eg. 'ALL' or specific script |
| ``admin_nopassword`` | ``false`` | - | Need no Password for sudo |
| ``admin_ansible_login`` | ``true`` | - | if ``admin: true`` and ``l3d_users__create_ansible: true`` your ssh keys will be added to ansible user |
| ``pubkeys`` | string or lookup | - | see examples |
| ``exklusive_pubkeys`` | ``true`` | - | delete all undefined ssh keys |
| ``password`` | password hash | - | See [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |

### Other variables
| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__create_ansible`` | ``true`` | Create User ansible |
| ``l3d_users__ansible_user_state`` | ``present`` | Create or delete user ansible |
| ``l3d_users__set_ansible_ssh_keys`` | ``false`` | Set SSH Keys for User ansible |
| ``l3d_users__ansible_ssh_keys`` | | SSH public Keys. One per line or as lookup |
| ``l3d_users__ansible_user_password`` | | Set optional Password for Ansible User, see [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``l3d_users__ansible_user_command`` | ``ALL`` | Commans with superpower for ansible user |
| ``l3d_users__ansible_user_nopassword`` | ``true`` | Allow superpowers without password for ansible user |
| ``l3d_users__limit_login`` | ``true`` | Only allow SSH login for specified users |
| ``l3d_users__sshd_port`` | ``22`` | Port for SSH |
| ``l3d_users__sshd_password_authentication`` | ``false`` | Allow login with Password |
| ``l3d_users__sshd_permitrootlogin`` | ``false`` | Allow login as root |
| ``l3d_users__sshd_manage_server_key_types`` | ``true`` | Manage Server SSH Key types |
| ``l3d_users__sshd_server_key_types`` | ``['ed25519']`` | List of supported SSH Key Types |
| ``l3d_users__sshd_manage_key_algorithmus`` | ``true`` | Manage SSH Key Algorythmins |
| ``l3d_users__sshd_key_algorithmus`` | ``['ssh-ed25519-cert-v01@openssh.com', 'ssh-ed25519', 'ecdsa-sha2-nistp521-cert-v01@openssh.com', 'ecdsa-sha2-nistp384-cert-v01@openssh.com', 'ecdsa-sha2-nistp256-cert-v01@openssh.com']`` | Used SSH Key Algorithms |
| ``l3d_users__sshd_manage_kex_algorithmus`` | ``true`` | Manage SSH Kex Algorythms |
| ``l3d_users__sshd_kex_algorithmus`` | ``['curve25519-sha256@libssh.org', 'diffie-hellman-group-exchange-sha256', 'diffie-hellman-group-exchange-sha1']`` | Used Kex Algorythms |
| ``l3d_users__sshd_manage_ciphers`` | ``true`` | Manage SSH Ciphers |
| ``l3d_users__sshd_ciphers`` | ``['chacha20-poly1305@openssh.com', 'aes256-gcm@openssh.com', 'aes256-ctr']`` | Used SSH Ciphers |
| ``l3d_users__sshd_manage_macs`` | ``true`` | Manage Used MACs |
| ``l3d_users__sshd_macs`` | ``['hmac-sha2-512-etm@openssh.com', 'hmac-sha2-256-etm@openssh.com', 'hmac-sha2-512']`` | Used MACs |
| ``l3d_users__sshd_xforwarding`` |``true`` | Enable X-Forwarding |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

## Requirements
+ See ``requirements.yml``
+ Installation:
```bash
ansible-galaxy collection install --requirements-file requirements.yml --upgrade
```
