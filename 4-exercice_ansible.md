%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice4

Set the group ownership of the directory `/opt/enigma/secret/` and each file contained
within the directory so that the group owner is `protected` for each host in `qa-servers`.

-------------------------------------------------------------------------------------

# 1. Linux Command:

You can use the following command directly on the Linux CentOS server to set the group ownership:
```bash
sudo chown -R :protected /opt/enigma/secret/
```

-------------------------------------------------------------------------------------

# 2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m command -a "chown -R :protected /opt/enigma/secret/"
```

-------------------------------------------------------------------------------------

# 3. Playbook - `set_ownership.yml`:

```yaml
---
- name: Set Group Ownership to protected
  hosts: qa-servers
  become: true
  tasks:
    - name: Set ownership of /opt/enigma/secret/
      file:
        path: /opt/enigma/secret/
        owner: root
        group: protected
        recurse: yes
        state: directory
```

Make sure to adjust the directory path `/opt/enigma/secret/` and the desired group name `protected`
according to your requirements.
