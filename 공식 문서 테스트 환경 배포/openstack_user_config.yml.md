

```
---
cidr_networks:
  management: 172.29.236.0/22
  tunnel: 172.29.240.0/22
  storage: 172.29.244.0/22

used_ips:
  - "172.29.236.1,172.29.236.50"
  - "172.29.240.1,172.29.240.50"
  - "172.29.244.1,172.29.244.50"
  - "172.29.248.1,172.29.248.50"

global_overrides:
  # The internal and external VIP should be different IPs, however they
  # do not need to be on separate networks.
  external_lb_vip_address: 192.168.0.143
  internal_lb_vip_address: 172.29.236.11
  management_bridge: "br-mgmt"
  provider_networks:
    - network:
        container_bridge: "br-mgmt"
        container_type: "veth"
        container_interface: "eth1"
        ip_from_q: "management"
        type: "raw"
        group_binds:
          - all_containers
          - hosts
        is_management_address: true
    - network:
        container_bridge: "br-vxlan"
        container_type: "veth"
        container_interface: "eth10"
        ip_from_q: "tunnel"
        type: "vxlan"
        range: "1:1000"
        net_name: "vxlan"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-vlan"
        container_type: "veth"
        container_interface: "eth12"
        host_bind_override: "eth12"
        type: "flat"
        net_name: "physnet1"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-vlan"
        container_type: "veth"
        container_interface: "eth11"
        type: "vlan"
        range: "101:200,301:400"
        net_name: "physnet2"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-storage"
        container_type: "veth"
        container_interface: "eth2"
        ip_from_q: "storage"
        type: "raw"
        group_binds:
          - glance_api
          - cinder_api
          - cinder_volume
          - nova_compute

###
### Infrastructure
###

# galera, memcache, rabbitmq, utility
shared-infra_hosts:
  infra1:
    ip: 172.29.236.11

# repository (apt cache, python packages, etc)
repo-infra_hosts:
  infra1:
    ip: 172.29.236.11

# load balancer
load_balancer_hosts:
  infra1:
    ip: 172.29.236.11

###
### OpenStack
###

# keystone
identity_hosts:
  infra1:
    ip: 172.29.236.11

# cinder api services
storage-infra_hosts:
  infra1:
    ip: 172.29.236.11

# glance
image_hosts:
  infra1:
    ip: 172.29.236.11

# placement
placement-infra_hosts:
  infra1:
    ip: 172.29.236.11

# nova api, conductor, etc services
compute-infra_hosts:
  infra1:
    ip: 172.29.236.11

# heat
orchestration_hosts:
  infra1:
    ip: 172.29.236.11

# horizon
dashboard_hosts:
  infra1:
    ip: 172.29.236.11

# neutron server, agents (L3, etc)
network_hosts:
  infra1:
    ip: 172.29.236.11

# nova hypervisors
compute_hosts:
  compute1:
    ip: 172.29.236.12
```


또는 cinder까지 추가
```
---
cidr_networks:
  management: 172.29.236.0/22
  tunnel: 172.29.240.0/22
  storage: 172.29.244.0/22

used_ips:
  - "172.29.236.1,172.29.236.50"
  - "172.29.240.1,172.29.240.50"
  - "172.29.244.1,172.29.244.50"
  - "172.29.248.1,172.29.248.50"

global_overrides:
  # The internal and external VIP should be different IPs, however they
  # do not need to be on separate networks.
  external_lb_vip_address: 192.168.0.143
  internal_lb_vip_address: 172.29.236.11
  management_bridge: "br-mgmt"
  provider_networks:
    - network:
        container_bridge: "br-mgmt"
        container_type: "veth"
        container_interface: "eth1"
        ip_from_q: "management"
        type: "raw"
        group_binds:
          - all_containers
          - hosts
        is_management_address: true
    - network:
        container_bridge: "br-vxlan"
        container_type: "veth"
        container_interface: "eth10"
        ip_from_q: "tunnel"
        type: "vxlan"
        range: "1:1000"
        net_name: "vxlan"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-vlan"
        container_type: "veth"
        container_interface: "eth12"
        host_bind_override: "eth12"
        type: "flat"
        net_name: "physnet1"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-vlan"
        container_type: "veth"
        container_interface: "eth11"
        type: "vlan"
        range: "101:200,301:400"
        net_name: "physnet2"
        group_binds:
          - neutron_linuxbridge_agent
    - network:
        container_bridge: "br-storage"
        container_type: "veth"
        container_interface: "eth2"
        ip_from_q: "storage"
        type: "raw"
        group_binds:
          - glance_api
          - cinder_api
          - cinder_volume
          - nova_compute

###
### Infrastructure
###

# galera, memcache, rabbitmq, utility
shared-infra_hosts:
  infra1:
    ip: 172.29.236.11

# repository (apt cache, python packages, etc)
repo-infra_hosts:
  infra1:
    ip: 172.29.236.11

# load balancer
load_balancer_hosts:
  infra1:
    ip: 172.29.236.11

###
### OpenStack
###

# keystone
identity_hosts:
  infra1:
    ip: 172.29.236.11

# cinder api services
storage-infra_hosts:
  infra1:
    ip: 172.29.236.11

# glance
image_hosts:
  infra1:
    ip: 172.29.236.11

# placement
placement-infra_hosts:
  infra1:
    ip: 172.29.236.11

# nova api, conductor, etc services
compute-infra_hosts:
  infra1:
    ip: 172.29.236.11

# heat
orchestration_hosts:
  infra1:
    ip: 172.29.236.11

# horizon
dashboard_hosts:
  infra1:
    ip: 172.29.236.11

# neutron server, agents (L3, etc)
network_hosts:
  infra1:
    ip: 172.29.236.11

# nova hypervisors
compute_hosts:
  compute1:
    ip: 172.29.236.12


storage_hosts:
  infra1:
    ip: 172.29.236.11
    container_vars:
      cinder_backends:
        limit_container_types: cinder_volume
        lvm:
          volume_group: cinder-volumes
          volume_driver: cinder.volume.drivers.lvm.LVMVolumeDriver
          volume_backend_name: LVM_iSCSI
          iscsi_ip_address: "172.29.244.13"
```