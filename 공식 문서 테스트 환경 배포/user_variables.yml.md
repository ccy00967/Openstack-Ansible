

```
---
# Copyright 2014, Rackspace US, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

###
### This file contains commonly used overrides for convenience. Please inspect
### the defaults for each role to find additional override options.
###

## Debug and Verbose options.
debug: false



neutron_plugin_type: ml2.lxb

neutron_ml2_drivers_type: "flat,vlan,vxlan"
neutron_plugin_base:
  - router
  - metering


## Set the service setup host
# The default is to use localhost (the deploy host where ansible runs),
# but any other host can be used. If using an alternative host with all
# required libraries in a venv (eg: the utility container) then the
# python interpreter needs to be set. If it is not, the default is to
# the system python interpreter.
# If you wish to use the first utility container in the inventory for
# all service setup tasks, uncomment the following.
#
#openstack_service_setup_host: "{{ groups['utility_all'][0] }}"
#openstack_service_setup_host_python_interpreter: "/openstack/venvs/utility-{{ openstack_release }}/bin/python"

## Installation method for OpenStack services
# Default option (source) is to install the OpenStack services using PIP
# packages. An alternative method (distro) is to use the distribution cloud
# repositories to install OpenStack using distribution packages
install_method: source

## Common Glance Overrides
# Set glance_default_store to "swift" if using Cloud Files backend
# or "rbd" if using ceph backend; the latter will trigger ceph to get
# installed on glance. If using a file store, a shared file store is
# recommended. See the OpenStack-Ansible install guide and the OpenStack
# documentation for more details.
# Note that "swift" is automatically set as the default back-end if there
# are any swift hosts in the environment. Use this setting to override
# this automation if you wish for a different default back-end.
# glance_default_store: file

## Ceph pool name for Glance to use
# glance_rbd_store_pool: images
# glance_rbd_store_chunk_size: 8

## Common Nova Overrides
# When nova_libvirt_images_rbd_pool is defined, ceph will be installed on nova
# hosts.
# nova_libvirt_images_rbd_pool: vms

# If you wish to change the dhcp_domain configured for both nova and neutron
# dhcp_domain: openstacklocal

## Common Glance Overrides when using a Swift back-end
# By default when 'glance_default_store' is set to 'swift' the playbooks will
# expect to use the Swift back-end that is configured in the same inventory.
# If the Swift back-end is not in the same inventory (ie it is already setup
# through some other means) then these settings should be used.
#
# NOTE: Ensure that the auth version matches your authentication endpoint.
#
# NOTE: If the password for glance_swift_store_key contains a dollar sign ($),
# it must be escaped with an additional dollar sign ($$), not a backslash. For
# example, a password of "super$ecure" would need to be entered as
# "super$$ecure" below.  See Launchpad Bug #1259729 for more details.
#
# glance_swift_store_auth_version: 3
# glance_swift_store_auth_address: "https://some.auth.url.com"
# glance_swift_store_user: "OPENSTACK_TENANT_ID:OPENSTACK_USER_NAME"
# glance_swift_store_key: "OPENSTACK_USER_PASSWORD"
# glance_swift_store_container: "NAME_OF_SWIFT_CONTAINER"
# glance_swift_store_region: "NAME_OF_REGION"

## Common Ceph Overrides
# ceph_mons:
#   - 10.16.5.40
#   - 10.16.5.41
#   - 10.16.5.42

## Custom Ceph Configuration File (ceph.conf)
# By default, your deployment host will connect to one of the mons defined above to
# obtain a copy of your cluster's ceph.conf.  If you prefer, uncomment ceph_conf_file
# and customise to avoid ceph.conf being copied from a mon.
# ceph_conf_file: |
#   [global]
#   fsid = 00000000-1111-2222-3333-444444444444
#   mon_initial_members = mon1.example.local,mon2.example.local,mon3.example.local
#   mon_host = 10.16.5.40,10.16.5.41,10.16.5.42
#   # optionally, you can use this construct to avoid defining this list twice:
#   # mon_host = {{ ceph_mons|join(',') }}
#   auth_cluster_required = cephx
#   auth_service_required = cephx


# By default, openstack-ansible configures all OpenStack services to talk to
# RabbitMQ over encrypted connections on port 5671. To opt-out of this default,
# set the rabbitmq_use_ssl variable to 'false'. The default setting of 'true'
# is highly recommended for securing the contents of RabbitMQ messages.
# rabbitmq_use_ssl: false

# RabbitMQ management plugin is enabled by default, the guest user has been
# removed for security reasons and a new userid 'monitoring' has been created
# with the 'monitoring' user tag. In order to modify the userid, uncomment the
# following and change 'monitoring' to your userid of choice.
# rabbitmq_monitoring_userid: monitoring


## Additional pinning generator that will allow for more packages to be pinned as you see fit.
## All pins allow for package and versions to be defined. Be careful using this as versions
## are always subject to change and updates regarding security will become your problem from this
## point on. Pinning can be done based on a package version, release, or origin. Use "*" in the
## package name to indicate that you want to pin all package to a particular constraint.
# apt_pinned_packages:
#   - { package: "lxc", version: "1.0.7-0ubuntu0.1" }
#   - { package: "libvirt-bin", version: "1.2.2-0ubuntu13.1.9" }
#   - { package: "rabbitmq-server", origin: "www.rabbitmq.com" }
#   - { package: "*", release: "MariaDB" }


## Environment variable settings
# This allows users to specify the additional environment variables to be set
# which is useful in setting where you working behind a proxy. If working behind
# a proxy It's important to always specify the scheme as "http://". This is what
# the underlying python libraries will handle best. This proxy information will be
# placed both on the hosts and inside the containers.

## Example environment variable setup:
## This is used by apt-cacher-ng to download apt packages:
# proxy_env_url: http://username:pa$$w0rd@10.10.10.9:9000/

## (1) This sets up a permanent environment, used during and after deployment:
# no_proxy_env: "localhost,127.0.0.1,{{ internal_lb_vip_address }},{{ external_lb_vip_address }},{% for host in groups['all_containers'] %}{{ hostvars[host]['management_address'] }}{% if not loop.last %},{% endif %}{% endfor %}"
# global_environment_variables:
#   HTTP_PROXY: "{{ proxy_env_url }}"
#   HTTPS_PROXY: "{{ proxy_env_url }}"
#   NO_PROXY: "{{ no_proxy_env }}"
#   http_proxy: "{{ proxy_env_url }}"
#   https_proxy: "{{ proxy_env_url }}"
#   no_proxy: "{{ no_proxy_env }}"
#
## (2) This is applied only during deployment, nothing is left after deployment is complete:
# deployment_environment_variables:
#   http_proxy: "{{ proxy_env_url }}"
#   https_proxy: "{{ proxy_env_url }}"
#   no_proxy: "localhost,127.0.0.1,{{ internal_lb_vip_address }},{{ external_lb_vip_address }},{% for host in groups['keystone_all'] %}{{ hostvars[host]['management_address'] }}{% if not loop.last %},{% endif %}{% endfor %}"


## SSH connection wait time
# If an increased delay for the ssh connection check is desired,
# uncomment this variable and set it appropriately.
#ssh_delay: 5


## HAProxy and keepalived
# All the previous variables are used inside a var, in the group vars.
# You can override the current keepalived definition (see
# group_vars/all/keepalived.yml) in your user space if necessary.
#
# Uncomment this to disable keepalived installation (cf. documentation)
# haproxy_use_keepalived: False
#
# HAProxy Keepalived configuration (cf. documentation)
# Make sure that this is set correctly according to the CIDR used for your
# internal and external addresses.
# If you've set external_lb_vip_address or internal_lb_vip_address to FQDN
# you must set these variables to the corresponding CIDRs for your setup.
# haproxy_keepalived_external_vip_cidr: "{{external_lb_vip_address}}/32"
# haproxy_keepalived_internal_vip_cidr: "{{internal_lb_vip_address}}/32"
# haproxy_keepalived_external_interface:
# haproxy_keepalived_internal_interface:

# Defines the default VRRP id used for keepalived with haproxy.
# Overwrite it to your value to make sure you don't overlap
# with existing VRRPs id on your network. Default is 10 for the external and 11 for the
# internal VRRPs
# haproxy_keepalived_external_virtual_router_id:
# haproxy_keepalived_internal_virtual_router_id:

# Defines the VRRP master/backup priority. Defaults respectively to 100 and 20
# haproxy_keepalived_priority_master:
# haproxy_keepalived_priority_backup:

# Keepalived default IP address used to check its alive status (IPv4 only)
# keepalived_external_ping_address: "193.0.14.129"
# keepalived_internal_ping_address: "193.0.14.129"
```


또는 cinder 볼륨 추가

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
root@ccy-controller:/home/ccy# cat /etc/openstack_deploy/user_variables.yml
---
# Copyright 2014, Rackspace US, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

###
### This file contains commonly used overrides for convenience. Please inspect
### the defaults for each role to find additional override options.
###

## Debug and Verbose options.
debug: false



neutron_plugin_type: ml2.lxb

neutron_ml2_drivers_type: "flat,vlan,vxlan"
neutron_plugin_base:
  - router
  - metering


# Cinder Backend 설정
cinder_backends:
  lvm:
    volume_driver: cinder.volume.drivers.lvm.LVMVolumeDriver
    volume_group: cinder-volumes
    volume_backend_name: lvm
    iscsi_protocol: iscsi
    target_helper: tgtadm

# Backend 활성화
cinder_backends_enabled:
  - lvm


## Set the service setup host
# The default is to use localhost (the deploy host where ansible runs),
# but any other host can be used. If using an alternative host with all
# required libraries in a venv (eg: the utility container) then the
# python interpreter needs to be set. If it is not, the default is to
# the system python interpreter.
# If you wish to use the first utility container in the inventory for
# all service setup tasks, uncomment the following.
#
#openstack_service_setup_host: "{{ groups['utility_all'][0] }}"
#openstack_service_setup_host_python_interpreter: "/openstack/venvs/utility-{{ openstack_release }}/bin/python"

## Installation method for OpenStack services
# Default option (source) is to install the OpenStack services using PIP
# packages. An alternative method (distro) is to use the distribution cloud
# repositories to install OpenStack using distribution packages
install_method: source

## Common Glance Overrides
# Set glance_default_store to "swift" if using Cloud Files backend
# or "rbd" if using ceph backend; the latter will trigger ceph to get
# installed on glance. If using a file store, a shared file store is
# recommended. See the OpenStack-Ansible install guide and the OpenStack
# documentation for more details.
# Note that "swift" is automatically set as the default back-end if there
# are any swift hosts in the environment. Use this setting to override
# this automation if you wish for a different default back-end.
# glance_default_store: file

## Ceph pool name for Glance to use
# glance_rbd_store_pool: images
# glance_rbd_store_chunk_size: 8

## Common Nova Overrides
# When nova_libvirt_images_rbd_pool is defined, ceph will be installed on nova
# hosts.
# nova_libvirt_images_rbd_pool: vms

# If you wish to change the dhcp_domain configured for both nova and neutron
# dhcp_domain: openstacklocal

## Common Glance Overrides when using a Swift back-end
# By default when 'glance_default_store' is set to 'swift' the playbooks will
# expect to use the Swift back-end that is configured in the same inventory.
# If the Swift back-end is not in the same inventory (ie it is already setup
# through some other means) then these settings should be used.
#
# NOTE: Ensure that the auth version matches your authentication endpoint.
#
# NOTE: If the password for glance_swift_store_key contains a dollar sign ($),
# it must be escaped with an additional dollar sign ($$), not a backslash. For
# example, a password of "super$ecure" would need to be entered as
# "super$$ecure" below.  See Launchpad Bug #1259729 for more details.
#
# glance_swift_store_auth_version: 3
# glance_swift_store_auth_address: "https://some.auth.url.com"
# glance_swift_store_user: "OPENSTACK_TENANT_ID:OPENSTACK_USER_NAME"
# glance_swift_store_key: "OPENSTACK_USER_PASSWORD"
# glance_swift_store_container: "NAME_OF_SWIFT_CONTAINER"
# glance_swift_store_region: "NAME_OF_REGION"

## Common Ceph Overrides
# ceph_mons:
#   - 10.16.5.40
#   - 10.16.5.41
#   - 10.16.5.42

## Custom Ceph Configuration File (ceph.conf)
# By default, your deployment host will connect to one of the mons defined above to
# obtain a copy of your cluster's ceph.conf.  If you prefer, uncomment ceph_conf_file
# and customise to avoid ceph.conf being copied from a mon.
# ceph_conf_file: |
#   [global]
#   fsid = 00000000-1111-2222-3333-444444444444
#   mon_initial_members = mon1.example.local,mon2.example.local,mon3.example.local
#   mon_host = 10.16.5.40,10.16.5.41,10.16.5.42
#   # optionally, you can use this construct to avoid defining this list twice:
#   # mon_host = {{ ceph_mons|join(',') }}
#   auth_cluster_required = cephx
#   auth_service_required = cephx


# By default, openstack-ansible configures all OpenStack services to talk to
# RabbitMQ over encrypted connections on port 5671. To opt-out of this default,
# set the rabbitmq_use_ssl variable to 'false'. The default setting of 'true'
# is highly recommended for securing the contents of RabbitMQ messages.
# rabbitmq_use_ssl: false

# RabbitMQ management plugin is enabled by default, the guest user has been
# removed for security reasons and a new userid 'monitoring' has been created
# with the 'monitoring' user tag. In order to modify the userid, uncomment the
# following and change 'monitoring' to your userid of choice.
# rabbitmq_monitoring_userid: monitoring


## Additional pinning generator that will allow for more packages to be pinned as you see fit.
## All pins allow for package and versions to be defined. Be careful using this as versions
## are always subject to change and updates regarding security will become your problem from this
## point on. Pinning can be done based on a package version, release, or origin. Use "*" in the
## package name to indicate that you want to pin all package to a particular constraint.
# apt_pinned_packages:
#   - { package: "lxc", version: "1.0.7-0ubuntu0.1" }
#   - { package: "libvirt-bin", version: "1.2.2-0ubuntu13.1.9" }
#   - { package: "rabbitmq-server", origin: "www.rabbitmq.com" }
#   - { package: "*", release: "MariaDB" }


## Environment variable settings
# This allows users to specify the additional environment variables to be set
# which is useful in setting where you working behind a proxy. If working behind
# a proxy It's important to always specify the scheme as "http://". This is what
# the underlying python libraries will handle best. This proxy information will be
# placed both on the hosts and inside the containers.

## Example environment variable setup:
## This is used by apt-cacher-ng to download apt packages:
# proxy_env_url: http://username:pa$$w0rd@10.10.10.9:9000/

## (1) This sets up a permanent environment, used during and after deployment:
# no_proxy_env: "localhost,127.0.0.1,{{ internal_lb_vip_address }},{{ external_lb_vip_address }},{% for host in groups['all_containers'] %}{{ hostvars[host]['management_address'] }}{% if not loop.last %},{% endif %}{% endfor %}"
# global_environment_variables:
#   HTTP_PROXY: "{{ proxy_env_url }}"
#   HTTPS_PROXY: "{{ proxy_env_url }}"
#   NO_PROXY: "{{ no_proxy_env }}"
#   http_proxy: "{{ proxy_env_url }}"
#   https_proxy: "{{ proxy_env_url }}"
#   no_proxy: "{{ no_proxy_env }}"
#
## (2) This is applied only during deployment, nothing is left after deployment is complete:
# deployment_environment_variables:
#   http_proxy: "{{ proxy_env_url }}"
#   https_proxy: "{{ proxy_env_url }}"
#   no_proxy: "localhost,127.0.0.1,{{ internal_lb_vip_address }},{{ external_lb_vip_address }},{% for host in groups['keystone_all'] %}{{ hostvars[host]['management_address'] }}{% if not loop.last %},{% endif %}{% endfor %}"


## SSH connection wait time
# If an increased delay for the ssh connection check is desired,
# uncomment this variable and set it appropriately.
#ssh_delay: 5


## HAProxy and keepalived
# All the previous variables are used inside a var, in the group vars.
# You can override the current keepalived definition (see
# group_vars/all/keepalived.yml) in your user space if necessary.
#
# Uncomment this to disable keepalived installation (cf. documentation)
# haproxy_use_keepalived: False
#
# HAProxy Keepalived configuration (cf. documentation)
# Make sure that this is set correctly according to the CIDR used for your
# internal and external addresses.
# If you've set external_lb_vip_address or internal_lb_vip_address to FQDN
# you must set these variables to the corresponding CIDRs for your setup.
# haproxy_keepalived_external_vip_cidr: "{{external_lb_vip_address}}/32"
# haproxy_keepalived_internal_vip_cidr: "{{internal_lb_vip_address}}/32"
# haproxy_keepalived_external_interface:
# haproxy_keepalived_internal_interface:

# Defines the default VRRP id used for keepalived with haproxy.
# Overwrite it to your value to make sure you don't overlap
# with existing VRRPs id on your network. Default is 10 for the external and 11 for the
# internal VRRPs
# haproxy_keepalived_external_virtual_router_id:
# haproxy_keepalived_internal_virtual_router_id:

# Defines the VRRP master/backup priority. Defaults respectively to 100 and 20
# haproxy_keepalived_priority_master:
# haproxy_keepalived_priority_backup:

# Keepalived default IP address used to check its alive status (IPv4 only)
# keepalived_external_ping_address: "193.0.14.129"
# keepalived_internal_ping_address: "193.0.14.129"
```