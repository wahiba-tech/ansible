%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice1

Become `ansible` user and then download http://software.xyzcorp.com/enigma.tgz
to `/tmp` on each host in qa-servers
and verify the sha256 checksum via http://software.xyzcorp.com/enigma-checksum.txt.

--------------------------------------------------------------------------------------

# 1.1. Shell File :


Create a shell file with the following content and make it executable:

```bash
!/bin/bash

sudo -u ansible wget http://software.xyzcorp.com/enigma.tgz -P /tmp

sudo -u ansible wget http://software.xyzcorp.com/enigma-checksum.txt -P /tmp

sudo -u ansible sha256sum -c /tmp/enigma-checksum.txt
```

--------------------------------------------------------------------------------------

# 1.2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m shell -a "sudo -u ansible wget http://software.xyzcorp.com/enigma.tgz\-P /tmp 

ansible wget http://software.xyzcorp.com/enigma-checksum.txt -P /tmp

ansible sha256sum -c /tmp/enigma-checksum.txt"
```

--------------------------------------------------------------------------------------

# 1.3. Playbook - `enigma.yml`:

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

--------------------------------------------------------------------------------------

# exercice2

write shell file, write command ansible adhoc command, write playbook 
to Extract `/tmp/enigma.tgz` to `/opt/` on all hosts in `qa-servers`

--------------------------------------------------------------------------------------

# 2.1. Shell File - `extract_enigma.sh`:

Create a shell file with the following content and make it executable:
```bash
#!/bin/bash

sudo tar -xzf /tmp/enigma.tgz -C /opt/
```

--------------------------------------------------------------------------------------

# 2.2. Ansible Ad-Hoc Command:

```
ansible qa-servers -m shell -a "sudo tar -xzf /tmp/enigma.tgz -C /opt/"
```

--------------------------------------------------------------------------------------

# 2.3. Playbook - `extract_enigma.yml`:

```
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

--------------------------------------------------------------------------------------

# exercice3

Update the line of text "DEPLOY_CODE" in `/opt/enigma/details.txt` to the "CODE_RED" on each server in `qa-servers`.

--------------------------------------------------------------------------------------

# 3.1. Linux Command:

You can use the following command directly on the Linux CentOS server:
```bash
sed -i 's/^DEPLOY_CODE/CODE_RED/' /opt/enigma/details.txt
```


--------------------------------------------------------------------------------------

# 3.2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m lineinfile -a "path=/opt/enigma/details.txt line='CODE_RED' regexp='^DEPLOY_CODE' state=present"
```

Make sure to adjust the file path `/opt/enigma/details.txt` to match your actual file path.

--------------------------------------------------------------------------------------

# 3.3. Playbook - `update_details.yml`:

```yaml
---
- name: Update Enigma Details
  hosts: qa-servers
  tasks:
    - name: Update details.txt file
      become: true
      replace:
        path: /opt/enigma/details.txt
        regexp: '^DEPLOY_CODE'
        replace: 'CODE_RED'
```

--------------------------------------------------------------------------------------

# exercice4

Set the group ownership of the directory `/opt/enigma/secret/` and each file contained
within the directory so that the group owner is `protected` for each host in `qa-servers`.

-------------------------------------------------------------------------------------

# 4.1. Linux Command:

You can use the following command directly on the Linux CentOS server to set the group ownership:
```bash
sudo chown -R :protected /opt/enigma/secret/
```

-------------------------------------------------------------------------------------

# 4.2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m command -a "chown -R :protected /opt/enigma/secret/"
```

-------------------------------------------------------------------------------------

# 4.3. Playbook - `set_ownership.yml`:

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

-------------------------------------------------------------------------------------

# exercice5

Delete the file `/opt/enigma/tmp/deployment-passwords.txt` from all servers.

-------------------------------------------------------------------------------------

# 5.1. Linux Command:

```bash
sudo rm -f /opt/enigma/tmp/deployment-passwords.txt
```

-------------------------------------------------------------------------------------

# 5.2. Ansible Ad-Hoc Command:

```bash
ansible all -m file -a "path=/opt/enigma/tmp/deployment-passwords.txt state=absent"
```

-------------------------------------------------------------------------------------

# 5.3. Playbook - `delete_file.yml`:

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
