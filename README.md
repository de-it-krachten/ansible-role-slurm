[![CI](https://github.com/de-it-krachten/ansible-role-slurm/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-slurm/actions?query=workflow%3ACI)


# ansible-role-slurm

Installs & configures Slurm (HPC cluster software)<br>
See https://slurm.schedmd.com/documentation.html 



## Dependencies

#### Roles
None

#### Collections
- community.general

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- RockyLinux 8
- OracleLinux 8
- AlmaLinux 8
- Debian 11 (Bullseye)
- Ubuntu 20.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# slurm cluster name
slurm_cluster_name: slurm_cl_1

# slurm accounting account
slurm_account_name: slurm

# slurm accounting users
slurm_users:
  - slurm

# Slurm firewall rules should be applied
slurm_firewall: true

# run slurm workers in configless mode
slurm_configless: auto

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

# MariaDB host
mariadb_db_host: localhost

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
slurm_slurmd_port: "6818"
slurm_slurmdbd_port: "6819"

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

# slurm partitions
slurm_partitions:
  - name: slurmall
    nodes: "{{ groups['slurm_nodes'] | map('regex_replace', '\\..*') | list }}"

# Set node to active
slurm_node_active: true

# Activate JWT authentication
slurm_jwt: false

# token to be used
slurm_jwt_token: jwt_hs256.key
</pre></code>


### vars/Debian.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm

# list of slurm packages
slurm_packages:
  slurmctld:
    - slurmctld
  slurmdbd:
    - slurmdbd
  slurmd:
    - slurmd
    - slurm-client
  client:
    - slurm-client
    - slurm-drmaa
</pre></code>

### vars/family-RedHat.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm

# list of slurm packages
slurm_packages:
  slurmctld:
    - slurm
    - slurm-devel
    - slurm-perlapi
    - slurm-slurmctld
    - slurm-torque
    # - slurm-slurmdbd
  slurmdbd:
    - slurm
    - slurm-devel
    - slurm-perlapi
    # - slurm-slurmctld
    # - slurm-torque
    - slurm-slurmdbd
  slurmd:
    - slurm
    - slurm-devel
    - slurm-perlapi
    - slurm-slurmd
  client:
    - slurm
    # - slurm-drmaa
</pre></code>

### vars/Ubuntu.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm-llnl

# slurm logging directory
slurm_log_dir: /var/log/slurm-llnl

# list of slurm packages
slurm_packages:
  slurmctld:
    - slurmctld
  slurmdbd:
    - slurmdbd
  slurmd:
    - slurmd
    - slurm-client
  client:
    - slurm-client
    - slurm-drmaa
</pre></code>



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'slurm'
  hosts: all
  gather_facts: yes
  become: yes
  vars:
    # ansible_python_interpreter: /usr/libexec/platform-python
    slurmd_package: "{{ 'slurm-slurmd' if ansible_os_family == 'RedHat' else 'slurmd' }}"
    custom_facts_additional:
      - name: slurm
        group: slurm_nodes
  pre_tasks:

    - name: Set hostname
      hostname:
        name: "{{ inventory_hostname | regex_replace('_','-') }}"
      when: ansible_virtualization_type not in [ 'docker', 'container', 'containerd' ]

    - name: Set slurm master ip
      set_fact:
        slurm_master_ip: "{{ hostvars[slurm_master_name]['ansible_default_ipv4']['address'] }}"
      when: slurm_uses_dns is defined and not slurm_uses_dns|bool

    - name: Install slurmd on nodes
      package:
        name: "{{ slurmd_package }}"
        state: present
      when: "'slurm_nodes' in group_names"

  roles:
    - { role: facts }
    - { role: epel, when: "ansible_os_family == 'RedHat'" }
    - { role: chrony, when: "github_actions is undefined" }
    - { role: hosts, when: "slurm_uses_dns is defined and not slurm_uses_dns|bool" }


- hosts: slurm_nodes
  gather_facts: yes
  become: yes
  tasks:

    - name: Display nodeinfo
      debug: var=ansible_local.slurm.nodeinfo

- hosts: slurm_db
  become: yes
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: "0666"
    mariadb_version: 10.4
    mariadb_root_password: 'Abcd1234'
    innodb:
      innodb_buffer_pool_size: 4096M
      innodb_log_file_size: 64M
      innodb_lock_wait_timeout: 900
    mariadb_db_name: testdb
    mariadb_db_user: testuser

  roles:
    - mariadb
    - munge
  tasks:
    - name: slurm
      include_role:
        name: slurm

- hosts: slurm_masters
  become: yes
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: "0666"
  roles:
    - munge
  tasks:
    - name: slurm
      include_role:
        name: slurm

- hosts: slurm_nodes
  become: yes
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: "0666"
  roles:
    - munge
  tasks:
    - name: slurm
      include_role:
        name: slurm
</pre></code>
