%title: ANSIBLE
%author: GHARBI Abdelaziz
# EXERCICE 01

write command ansible adhoc command, write playbook to do these command

mkdir /home/vagrant/ex1

touch /home/vagrant/ex1/txt.txt

chmod 600 /home/vagrant/ex1/txt.txt 

echo "hello 1st line" >/home/vagrant/ex1/txt.txt 

echo "hello 1st line" >>/home/vagrant/ex1/txt.txt 

-------------------------------------------------------------------------------------

# Ansible Ad-Hoc Command:

```
ansible all -i inventory_file -m shell -a "mkdir /home/vagrant/ex1"

ansible all -i inventory_file -m file -a "path=/home/vagrant/ex1/txt.txt state=touch"

ansible all -i inventory_file -m file -a "path=/home/vagrant/ex1/txt.txt mode=600"

ansible all -i inventory_file -m shell -a "echo 'hello 1st line' >/home/vagrant/ex1/txt.txt"

ansible all -i inventory_file -m shell -a "echo 'hello 1st line' >>/home/vagrant/ex1/txt.txt"
```

Replace `inventory_file` with the path to your Ansible inventory file.

-------------------------------------------------------------------------------------

# Playbook:

```
- name: Create and modify files
  hosts: all
  gather_facts: false
  tasks:
    - name: Create directory
      file:
        path: /home/vagrant/ex1
        state: directory

    - name: Create empty file
      file:
        path: /home/vagrant/ex1/txt.txt
        state: touch

    - name: Set file permissions
      file:
        path: /home/vagrant/ex1/txt.txt
        mode: '0600'

    - name: Write to file (overwrite)
      copy:
        dest: /home/vagrant/ex1/txt.txt
        content: "hello 1st line"

    - name: Append to file
      copy:
        dest: /home/vagrant/ex1/txt.txt
        content: "hello 1st line"
        remote_src: yes
        insertafter: EOF
```

Run the playbook with `ansible-playbook -i inventory_file playbook.yaml`,
replacing `inventory_file` with the path to your Ansible inventory file.

-------------------------------------------------------------------------------------

# EXERCICE 02 

```
scp /home/vagrant/ex1/txt.txt vagrant@client:/home/vagrant
```

-------------------------------------------------------------------------------------

# Ansible Ad-Hoc Command:

```bash
ansible all -i inventory_file -m copy -a "src=/home/vagrant/ex1/txt.txt dest=/home/vagrant/txt.txt owner=vagrant group=vagrant mode=0644"
```

Replace `inventory_file` with the path to your Ansible inventory file.

-------------------------------------------------------------------------------------

# Playbook:

```
- name: Copy file to client
  hosts: all
  gather_facts: false
  tasks:
    - name: Copy file
      copy:
        src: /home/vagrant/ex1/txt.txt
        dest: /home/vagrant/txt.txt
        owner: vagrant
        group: vagrant
        mode: '0644'
```

Run the playbook with `ansible-playbook -i inventory_file playbook.yaml`,
replacing `inventory_file` with the path to your Ansible inventory file.

-------------------------------------------------------------------------------------

# EXERCICE 03

Command in Linux (CentOS):

```
systemctl start nginx

systemctl enable nginx

systemctl status nginx

hostnamectl set-hostname DBnode
```

-------------------------------------------------------------------------------------

# Ansible Ad-Hoc Command:

```
ansible all -i inventory_file -m systemd -a "name=nginx state=started"

ansible all -i inventory_file -m systemd -a "name=nginx enabled=yes"

ansible all -i inventory_file -m systemd -a "name=nginx state=started"

ansible all -i inventory_file -m command -a "hostnamectl set-hostname DBnode"
```

Replace `inventory_file` with the path to your Ansible inventory file.

-------------------------------------------------------------------------------------

# Playbook:

```
- name: Manage nginx and hostname
  hosts: all
  gather_facts: false
  tasks:
    - name: Start nginx
      systemd:
        name: nginx
        state: started

    - name: Enable nginx at boot
      systemd:
        name: nginx
        enabled: yes

    - name: Check nginx status
      systemd:
        name: nginx
        state: started

    - name: Set hostname
      command: hostnamectl set-hostname DBnode
```

Run the playbook with `ansible-playbook -i inventory_file playbook.yaml`, replacing `inventory_file` with the path to your Ansible inventory file.
