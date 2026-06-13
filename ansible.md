# ansible
    sudo apt install ansible
    pip install ansible
    
    # show version
    ansible --version

    copy sshkey on server

# ansible server
  mkdir workspace
  /etc/ansible/ansible.cfg ===>> old directory has config file
 	you can make configuration everywhere

## vim inventory.ini
### First way
    [all]

    debian	ansible_host=191.168.1.105
            ansible_user=behnam
            ansible_ssh_password=123
            ansible_port=22
            ansible_connection=ssh
            ansible_ssh_private_key_file=/home/behnam/.ssh/id_bhnm

    RHEL	  ansible_host=191.168.1.110
            ansible_user=milad
            ansible_ssh_password=321
            ansible_port=2222
            ansible_connection=ssh
            ansible_ssh_private_key_file=/home/behnam/.ssh/id_bhnm
        
### second way
    [all]
        server1 ansible_host=192.168.1.105
        server2 ansible_host=192.168.1.110
        server3 ansible_host=192.168.1.115
    
    [web]
        server1
    
    [db]
        server2
    
    [lb]
        server3
    
    [all:vars]
        ansible_user=behnam
        ansible_port=22
        ansible_connection=ssh
        ansible_ssh_private_key_file=/home/behnam/.ssh/id_bhnm

## vim inventory.yaml
    all:
      children:
        webservers:
          hosts:
            192.168.1.105:
              ansible_port: 22
            192.168.1.110:
              ansible_port: 2222
          vars:
            http_port: 80
            
        dbservers:
          hosts:
            db1.behnam.local:
              db_user: admin
              db_pass: 123
            db2.behnam.local:
              db_user: admin
              db_pass: 321
              db_host: 192.168.1.110
              db_port: 22

## command
  # check *.ini for servers you have
  ansible --list-hosts all -i /home/behnam/workspace/inventory.ini
  ansible --list-hosts all, db, web  inventory.ini

# ansible ad-hoc command
  
  # -m ===>> module
  # -i ===>> inventory
  # -a ===>> execute

  # module ping
  ansible -m ping all -i inventory.ini
  ansible -m ping db -i inventory.ini
  ansible -m ping web -i inventory.ini
  
  # module command
  ansible -m command -a "uptime" -i inventory.ini all
	ansible -m command -a "uname -a" -i inventory.ini all
	ansible -m command -a "sudo reboot" -i inventory.ini all
  ansible -m command -a "touch /tmp/backup" -i inventory.ini all
	ansible -m command -a "cp /etc/passwd /tmp/passwd.bk" -i inventory.ini all

  # module copy
  # copy file from ansible server to server
  vim file1.txt
	  behnam	
	ansible all -m copy -a "src=/home/behnam/file1.txt dest=/home/behnam/file1.txt" -i inventory.ini

  # --become ===>> switch user
  # --becomeuser root ===>> choose your user
  # --become-method sudo or su ===>> change method
  # --ask-become-pass ===>> ask password sudo
  # -K ===>> ask password sudo
	ansible all -m copy -a "src=/home/behnam/file1.txt dest=/home/milad/file1.txt" -i inventory.ini --become --become-user root --become-method sudo

  ansible all -m copy -a "src=/home/behnam/file1.txt dest=/home/milad/file1.txt" -i inventory.ini --become --become-user root --become-method sudo --ask-become-pass

  # module apt
  # state = present ===>> install
  # state = absent ===>> remove
  # -v ===>> verbos
	ansible all -m apt -a "name=nginx state=present" --become -i inventory.ini
  ansible all -m apt -a "name=nginx state=present" --become -i inventory.ini -v
	ansible all -m apt -a "name=nginx=4.1.4 state=present" --become -i inventory.ini
	ansible all -m apt -a "name=nginx state=absent " --become -i inventory.ini
	ansible all -m apt -a "update_cache=yes upgrade=yes"  --become -i inventory.ini
  andible all -m iptables -i inventory.ini

  # module command & shell & row
	modul command why we have modul shell ===>> you can't redirect append pipe ; && ||
	row modul ===>> can command in no have python system like sisco
  
  # module shell
  ansible all -m shell -a "cat /etc/passwd | wc -l" --become -i inventory.ini
	ansible all -m shell -a "mkdir /backup" --become -i inventory.ini
	ansible all -m shell -a "cp -r /etc /backup" --become -i inventory.ini
  ansible all -m shell -a "du -h /" --become -i inventory.ini
  ansible all -m shell -a "du -h " --become -i inventory.ini
  ansible all -m shell -a "tar -czf /tmp/log_backup_$(date +%Y%m%d).tar.gz /var/log" --become -i inventory.ini
  ansible all -m shell -a "find /var/log -type f -mtime +30 -delete" --become -i inventory.ini
  ansible all -m shell -a "systemctl restart nginx" --become -i inventory.ini

  # module setup
  # all information about os (ssh, disk, network, lvm)
  ansible all -m setup --become -i inventory.ini
  ansible all -m setup -a "filte=ansible_distribution" --become -i inventory.ini
  ansible all -m setup -a "filte=ansible_memory*" --become -i inventory.ini
  ansible all -m setup -a "filte=ansible_processor*" --become -i inventory.ini
  ansible all -m setup -a "filte=ansible_mounts" --become -i inventory.ini
  
  # module doc
  # show all document
  ansible-dock apt
  ansible-dock shell
  ansible-dock file
  ansible-dock copy


  # playbook
  ansible-playbook
  touch install-nfinx.yaml
    ---
    - name: Install nginx web server
      hostas: all
      become: yes
      tasks:
        - name: Update apt cache
          ansible.builtin.apt:
            update-cache: yes
        
        - name: Ensure nginx package is installed
          ansible.builtin.apt:
            name: nginx
            state: present
        
        - name: Ensure nginx package is installed
          ansible.builtin.apt:
            name: nginx
            state: absent

        - name: Copy file from local to server
          ansible.builtin.copy:
            src: /home/behnam/Document/file1
            dest: /home/debian/Document/file1
        
        - name: Copy file from server to server
          ansible.builtin.copy:
            src: /home/behnam/Document/file2
            dest: /home/debian/Document/file2
            remote_src: yes

        - name: restart, stop, start, reload service
          ansibel.builtin.sercice:
            name: ngix
            state: started
            state: stopped
            state: restarted
            state: reloaded
            state: enabled

        - name: Make directory
          shell:
            cmd: mkdir /tmp/file1
          ignore_errors: yes

        - name: Print status
          debug:
            msg: nginx installed
```
        - name: Execute shell command
          shell:
            cmd: df -h
          register: shell_output

        - name: Display shell output
          debug:
            msg: "The command output is: {{ shell_output.stdout }}
```        
        - name: Pint boot image
          debug:
            msg: "The boot image is: {{ ansible_cmdline.BOOT_IMAGE }}"
        
        - name: Print server ips
          debug:
            msg: "The ips address is: {{ ansible_all_ipv4_addresses }}"
```
        - name: Check file exist
          command: ls /tmp/file1
            register: file_check
            ignore_eerrors: yes

        - name: Print file exist
          debug:
            msg: "file exist"
          when: file_check.rc == 0

        - name: Print file does not exist
          debug:
            msg: "file does not exist"
          when: file_check.rc != 0
```


   save the file
   ansible-palybook install_nginx.yaml -i inventory.ini


   -i inventory.ini