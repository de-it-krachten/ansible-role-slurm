[![CI](https://github.com/de-it-krachten/ansible-role-slurm/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-slurm/actions?query=workflow%3ACI)


# ansible-role-slurm

Installs & configures Slurm (HPC cluster software)<br>
See https://slurm.schedmd.com/documentation.html 



## Dependencies

#### Roles
None

#### Collections
None

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

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

# Should slurm.conf be updated
slurm_conf_update: true

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

# Slurm ports
slurm_slurmctld_port: "6817"
slurm_slurmd_port: "6818"
slurm_slurmdbd_port: "6819"

# Set node to active
slurm_node_active: true

# Activate JWT authentication
slurm_jwt: false

# token to be used
slurm_jwt_token: jwt_hs256.key

# slurm partitions
slurm_partitions:
  - name: slurmall
    nodes: "{{ groups['slurm_nodes'] | map('regex_replace', '\\..*') | list }}"

# Reboot/resume commands
# slurm_reboot_program:
# slurm_resume_program:
# slurm_resume_timeout:
</pre></code>

### defaults/Debian.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm

# Slurm daemon config file
slurm_parm_file: /etc/default/slurmd

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

### defaults/family-RedHat-8.yml
<pre><code>
# Slurm DRMAA RPM
slurm_drmaa_rpm: >-
  https://github.com/natefoo/slurm-drmaa/releases/download/1.1.4/slurm-drmaa-1.1.4.-20.11.el8.x86_64.rpm
</pre></code>

### defaults/family-RedHat-9.yml
<pre><code>
# Slurm DRMAA RPM
slurm_drmaa_rpm: >-
  https://github.com/natefoo/slurm-drmaa/releases/download/1.1.4/slurm-drmaa-1.1.4.-22.05.el9.x86_64.rpm
</pre></code>

### defaults/family-RedHat.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm

# Slurm daemon config file
slurm_parm_file: /etc/sysconfig/slurmd

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

### defaults/family-Suse.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm

# Slurm daemon config file
slurm_parm_file: /etc/sysconfig/slurmd

# list of slurm packages
slurm_packages:
  slurmctld:
    - slurm
    - slurm-devel
    - slurm-torque
    - slurm-munge
  slurmdbd:
    # - slurm
    - slurm-devel
    - slurm-slurmdbd
    - slurm-munge
  slurmd:
    # - slurm
    - slurm-node
    - slurm-munge
  client:
    - slurm
    # - slurm-drmaa
</pre></code>

### defaults/Ubuntu-18.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm-llnl

# slurm logging directory
slurm_log_dir: /var/log/slurm-llnl
</pre></code>

### defaults/Ubuntu-20.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm-llnl

# slurm logging directory
slurm_log_dir: /var/log/slurm-llnl
</pre></code>

### defaults/Ubuntu-22.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm
</pre></code>

### defaults/Ubuntu-24.yml
<pre><code>
# slurm configuration directory
slurm_conf_dir: /etc/slurm

# slurm logging directory
slurm_log_dir: /var/log/slurm
</pre></code>

### defaults/Ubuntu.yml
<pre><code>
slurm_drmaa_repo_url: >-
  https://ppa.launchpadcontent.net/natefoo/slurm-drmaa/ubuntu

slurm_drmaa_key_url: >-
  https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x8de68488997c5c6ba19021136f2cc56412788738

# Slurm daemon config file
slurm_parm_file: /etc/default/slurmd

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
    - slurm-drmaa1
</pre></code>


### vars/override.yml
<pre><code>
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



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'slurm'
  hosts: all
  gather_facts: 'yes'
  become: 'yes'
  vars:
    slurmd_package:
      RedHat: slurm-slurmd
      Debian: slurmd
      Suse: slurm-node
    custom_facts_additional:
      - name: slurm
        group: slurm_nodes
  pre_tasks:
    - name: Set hostname
      hostname:
        name: '{{ inventory_hostname | regex_replace(''_'',''-'') }}'
      when: ansible_virtualization_type not in [ 'docker', 'container', 'containerd'
        ]
    - name: Set slurm master ip
      set_fact:
        slurm_master_ip: '{{ hostvars[slurm_master_name][''ansible_default_ipv4''][''address'']
          }}'
      when:
        - slurm_master_ip is undefined
        - slurm_uses_dns is defined and not slurm_uses_dns | bool
    - name: Install slurmd on nodes
      package:
        name: '{{ slurmd_package[ansible_os_family] }}'
        state: present
      when: '''slurm_nodes'' in group_names'
  roles:
    - role: deitkrachten.facts
    - role: deitkrachten.epel
      when: ansible_os_family == 'RedHat'
    - role: deitkrachten.chrony
      when: github_actions is undefined
    - role: deitkrachten.hosts
      hosts_adapter: '{{ ''eth1'' if lookup(''env'', ''MOLECULE_DRIVER_NAME'') ==
        ''vagrant'' else ''default'' }}'
      when: slurm_uses_dns is defined and not slurm_uses_dns|bool
- hosts: slurm_nodes
  gather_facts: 'yes'
  become: 'yes'
  tasks:
    - name: Display nodeinfo
      debug: var=ansible_local.slurm.nodeinfo
- hosts: slurm_db
  become: 'yes'
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: '0666'
    mariadb_release: '10.11'
    mariadb_root_password: Abcd1234
    innodb:
      innodb_buffer_pool_size: 4096M
      innodb_log_file_size: 64M
      innodb_lock_wait_timeout: 900
    mariadb_db_name: testdb
    mariadb_db_user: testuser
  roles:
    - deitkrachten.mariadb
    - deitkrachten.munge
  tasks:
    - name: slurm
      include_role:
        name: slurm
- hosts: slurm_masters
  become: 'yes'
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: '0666'
  roles:
    - deitkrachten.munge
  tasks:
    - name: slurm
      include_role:
        name: slurm
- hosts: slurm_nodes
  become: 'yes'
  vars:
    munge_key: tests/munge.key
    munge_socket_mode: '0666'
  roles:
    - deitkrachten.munge
  tasks:
    - name: slurm
      include_role:
        name: slurm
</pre></code>
