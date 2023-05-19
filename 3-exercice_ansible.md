%title: ANSIBLE
%author: GHARBI Abdelaziz
# exercice3

Update the line of text "DEPLOY_CODE" in `/opt/enigma/details.txt` to the "CODE_RED" on each server in `qa-servers`.

--------------------------------------------------------------------------------------

# 1. Linux Command:

You can use the following command directly on the Linux CentOS server:
```bash
sed -i 's/^DEPLOY_CODE/CODE_RED/' /opt/enigma/details.txt
```


--------------------------------------------------------------------------------------

# 2. Ansible Ad-Hoc Command:

```bash
ansible qa-servers -m lineinfile -a "path=/opt/enigma/details.txt line='CODE_RED' regexp='^DEPLOY_CODE' state=present"
```

Make sure to adjust the file path `/opt/enigma/details.txt` to match your actual file path.

--------------------------------------------------------------------------------------

# 3. Playbook - `update_details.yml`:

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

