[![CI](https://github.com/de-it-krachten/ansible-role-slurm/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-slurm/actions?query=workflow%3ACI)


# ansible-role-slurm

Installs & configure slurm


Platforms
--------------

Supported platforms

- CentOS 7
- CentOS 8
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Debian 10 (Buster)
- Debian 11 (Bullseye)



Role Variables
--------------
<pre><code>
# slurm account
slurm_account_name: galaxy

# run slurm workers in configless mode
slurm_configless: false

# slurm roles
slurm_roles: []

# slurm group2role mapping (members of these groups will be assigned these roles)
slurm_group2role_mapping:
  - group: slurm_masters
    role: slurmctld
  - group: slurm_db
    role: slurmdbd
  - group: slurm_nodes
    role: slurmd

# Should root be disable from slurm
slurm_disable_root: 'YES'

# slurm user w/ UID
slurm_user: slurm
slurm_uid: 64030
# slurm group w/ GID
slurm_group: slurm
slurm_gid: 64030

# Slurm directories
slurm_dirs:
  slurmctld:
    - { path: "{{ slurm_conf_dir }}" }
    - { path: "{{ slurm_log_dir }}" }
    - { path: /var/spool/slurmctld }
  slurmdbd:
    - { path: "{{ slurm_conf_dir }}" }
    - { path: "{{ slurm_log_dir }}" }
  slurmd:
    - { path: "{{ slurm_conf_dir }}" }
    - { path: "{{ slurm_log_dir }}" }
    - { path: /var/spool/slurmd }

# Slurm ports
slurm_slurmctld_port: "6817"
slurm_slurmd_port:    "6818"
slurm_slurmdbd_port:  "6819"

# Slurm firewall ports
slurm_firewall_ports:
  slurmctld:
    - port: "{{ slurm_slurmctld_port }}"
      proto: tcp
    - port: '60001-63000'
      proto: tcp
  slurmd:
    - port: "{{ slurm_slurmd_port }}"
      proto: tcp
    - port: '60001-63000'
      proto: tcp
  slurmdbd:
    - port: "{{ slurm_slurmdbd_port }}"
      proto: tcp
</pre></code>


Example Playbook
----------------

<pre><code>
- name: Converge
  hosts: all
  gather_facts: yes
  become: yes
  vars:
    #ansible_python_interpreter: /usr/libexec/platform-python
    custom_facts_additional:
      - name: slurm
        group: slurm_nodes

  roles:
    - common
    - robertdebock.powertools
    - robertdebock.epel
    - chrony
    - { role: firewalld, when: "ansible_os_family == 'RedHat'" }
    - { role: ufw, when: "ansible_os_family == 'Debian'" }
  tasks:


    - name: Set hostname
      hostname:
        name: "{{ inventory_hostname | regex_replace('_','-') }}"

    - block:

        - name: Register all masters + nodes in /etc/hosts
          lineinfile:
            line: "{{ hostvars[item]['ansible_eth1']['ipv4']['address'] }} {{ item }}"
            path: /etc/hosts
          loop: "{{ groups['all'] }}"

        - name: Set slurm master ip
          set_fact:
            slurm_master_ip: "{{ hostvars[slurm_master_name]['ansible_eth1']['ipv4']['address'] }}"

      when: slurm_uses_dns is defined and not slurm_uses_dns|bool

     

- hosts: slurm_nodes
  gather_facts: yes
  become: yes
  tasks:
    - name: Create custom facts
      file:
        path: /etc/ansible/facts.d
        state: directory
        mode: "0755"
    - name: Install slurm node package (RedHat/CentOS)
      dnf:
        name: slurm-slurmd
        state: present
      when: ansible_os_family == 'RedHat'
    - name: Install slurm node package (Ubuntu/Debian)
      apt:
        name: slurmd
        state: present
      when: ansible_os_family == 'Debian'
    - name: Create custom fact
      copy:
        src: templates/facts/slurm.fact.j2
        dest: /etc/ansible/facts.d/slurm.fact
        mode: '0755'
    - name: Obtain node information
      setup:
    - name: Display nodeinfo
      debug: var=ansible_local.slurm.nodeinfo

- hosts: slurm_db
  become: yes
  vars:
    munge_key: tests/munge.key
  roles:
    - mariadb
    - munge
    #- ansible-role-slurm

- hosts: slurm_masters
  become: yes
  vars:
    munge_key: tests/munge.key
  roles:
    - munge
    #- ansible-role-slurm

- hosts: slurm_nodes
  become: yes
  vars:
    munge_key: tests/munge.key
  roles:
    - munge
    #- ansible-role-slurm
</pre></code>
