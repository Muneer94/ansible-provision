# Ansible Role: Provision Servers
===============================

### Example Playbook
--------------------

```
---

- hosts: servers
  become: yes
  become_method: sudo

  vars:
    packages:
      - nano
      - vim
      - htop
      - iotop
      - git
      - ntp
      - locate
      - lxc
      - lxctl
      - lxc-templates
      - lwp
      - bridge-utils
    set_timezone: Asia/Kolkata
    set_config:
      #Max value it can handle is "net.netfilter.nf_conntrack_max = 2097152" change based on your requirement.
      net.netfilter.nf_conntrack_max: 524288

      #Max value it can handle is "net.ipv4.tcp_max_tw_buckets = 262144" change based on your requirement.
      net.ipv4.tcp_max_tw_buckets:  262144

      #Maximum no of outbound sockets a host can create from a particular I.P. address
      net.ipv4.ip_local_port_range: 15000 61000

      #When a TCP Connection closes a port it cannot be reused immediately so need to reduce to time wait interval to 15 - 30 seconds
      net.ipv4.tcp_fin_timeout: 15

      #Process waits for socket acivity fo 300 seconds
      net.ipv4.tcp_keepalive_time:  300

      #Maximum Number of backlogged sockets
      net.core.somaxconn: 4096

      #If the interface is accepting connections faster while the kernel is slower processing them, then those connections are buffered
      net.core.netdev_max_backlog:  65536

      #Limiting orphan processes so that it would not be using more resources
      net.ipv4.tcp_max_orphans: 16384

      #TCP saves various connection metrics in cache when connection closes, we are just disabling it
      net.ipv4.tcp_no_metrics_save: 1

      #Allows a connection in wait state to be reused
      net.ipv4.tcp_tw_reuse:  1

      #Enabling Swappiness
      vm.swappiness:  10
    user: "*"
    set_pam:
      soft: 100000
      hard: 655350
    set_host: localhost
    set_port: 23397

  roles:
    - deploy.provision

```
