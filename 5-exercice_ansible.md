%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice5

Delete the file `/opt/enigma/tmp/deployment-passwords.txt` from all servers.

-------------------------------------------------------------------------------------

# 1. Linux Command:

```bash
sudo rm -f /opt/enigma/tmp/deployment-passwords.txt
```

-------------------------------------------------------------------------------------

# 2. Ansible Ad-Hoc Command:

```bash
ansible all -m file -a "path=/opt/enigma/tmp/deployment-passwords.txt state=absent"
```

-------------------------------------------------------------------------------------

# 3. Playbook - `delete_file.yml`:

```yaml
---
- name: Delete deployment-passwords.txt file
  hosts: all
  become: true
  tasks:
    - name: Delete file
      file:
        path: /opt/enigma/tmp/deployment-passwords.txt
        state: absent
```

Make sure to adjust the file path `/opt/enigma/tmp/deployment-passwords.txt` to match your actual file path.
