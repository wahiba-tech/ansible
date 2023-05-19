%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice1

Become `ansible` user and then download http://software.xyzcorp.com/enigma.tgz
to `/tmp` on each host in qa-servers
and verify the sha256 checksum via http://software.xyzcorp.com/enigma-checksum.txt.

--------------------------------------------------------------------------------------

# 1. Shell File :


Create a shell file with the following content and make it executable:

```bash
!/bin/bash

sudo -u ansible wget http://software.xyzcorp.com/enigma.tgz -P /tmp

sudo -u ansible wget http://software.xyzcorp.com/enigma-checksum.txt -P /tmp

sudo -u ansible sha256sum -c /tmp/enigma-checksum.txt
```

--------------------------------------------------------------------------------------

# 2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m shell -a "sudo -u ansible wget http://software.xyzcorp.com/enigma.tgz\-P /tmp 

ansible wget http://software.xyzcorp.com/enigma-checksum.txt -P /tmp

ansible sha256sum -c /tmp/enigma-checksum.txt"
```

--------------------------------------------------------------------------------------

# 3. Playbook - `enigma.yml`:

```yaml
---
- name: Download and Verify Enigma Software
  hosts: qa-servers
  become: true
  become_user: ansible
  tasks:
    - name: Download Enigma Software
      get_url:
        url: http://software.xyzcorp.com/enigma.tgz
        dest: /tmp/enigma.tgz

    - name: Download Checksum File
      get_url:
        url: http://software.xyzcorp.com/enigma-checksum.txt
        dest: /tmp/enigma-checksum.txt

    - name: Verify SHA256 Checksum
      command: sha256sum -c /tmp/enigma-checksum.txt
      args:
        chdir: /tmp
      register: checksum_result
      changed_when: false

    - name: Display Checksum Verification Result
      debug:
        msg: "Checksum verification: {{ 'OK' if checksum_result.rc == 0 else 'Failed' }}"
```

Make sure to replace the URL `http://software.xyzcorp.com` with the correct URL for your scenario.
Adjust the file paths and any other details as per your specific requirements.
