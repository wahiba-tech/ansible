%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice2

write shell file, write command ansible adhoc command, write playbook 
to Extract `/tmp/enigma.tgz` to `/opt/` on all hosts in `qa-servers`

--------------------------------------------------------------------------------------

# 1. Shell File - `extract_enigma.sh`:
Create a shell file with the following content and make it executable:
```bash
#!/bin/bash

sudo tar -xzf /tmp/enigma.tgz -C /opt/
```

--------------------------------------------------------------------------------------

# 2. Ansible Ad-Hoc Command:
```bash
ansible qa-servers -m shell -a "sudo tar -xzf /tmp/enigma.tgz -C /opt/"
```

--------------------------------------------------------------------------------------

# 3. Playbook - `extract_enigma.yml`:
```yaml
---
- name: Extract Enigma Software
  hosts: qa-servers
  become: true
  tasks:
    - name: Extract enigma.tgz to /opt/
      unarchive:
        src: /tmp/enigma.tgz
        dest: /opt/
        remote_src: yes
```

Make sure to have the `/tmp/enigma.tgz` file available on each target host.
Adjust the file paths and any other details as per your specific requirements.
