# ansible-openstack-compute-service-controller

An [Ansible](https://www.ansible.com) role to install/configure
[OpenStack Compute Service Controller](https://docs.openstack.org/ocata/install-guide-ubuntu/nova-controller-install.html)

## Requirements

None

## Role Variables

```yaml
---
# defaults file for ansible-openstack-compute-service-controller

openstack_compute_service_controller_endpoint_services:
  - name: 'nova'
    type: 'compute'
    description: "Nova Service"
    api_port: 8774
    api_version: 'v2.1'
  - name: 'placement'
    type: 'placement'
    description: "Placement Service"
    api_port: 8778

# keystone_authtoken
openstack_compute_service_controller_keystone_authtoken:
  auth_type: 'password'
  auth_uri: '{{ openstack_compute_service_controller_keystone_service_endpoint_url }}:5000'
  auth_url: '{{ openstack_compute_service_controller_keystone_service_endpoint_url }}:35357'
  memcached_servers:
    - 'localhost'
  password: "{{ openstack_compute_service_controller_nova_user_info['password'] }}"
  project_domain_name: "{{ openstack_compute_service_controller_nova_user_info['domain_id'] }}"
  project_name: "{{ openstack_compute_service_controller_nova_user_info['project'] }}"
  user_domain_name: "{{ openstack_compute_service_controller_nova_user_info['domain_id'] }}"
  username: "{{ openstack_compute_service_controller_nova_user_info['name'] }}"

openstack_compute_service_controller_keystone_service_endpoint_region: 'RegionOne'
openstack_compute_service_controller_keystone_service_endpoint_url: 'http://{{ ansible_hostname }}'

# Management IP Info
openstack_compute_service_controller_management_interface: 'enp0s8'
openstack_compute_service_controller_management_ip: "{{ hostvars[inventory_hostname]['ansible_'+openstack_compute_service_controller_management_interface]['ipv4']['address'] }}"

openstack_compute_service_controller_nova_db_host: 'localhost'
openstack_compute_service_controller_nova_db_pass: 'nova'

openstack_compute_service_controller_nova_user_info:
  description: 'Nova user'
  domain_id: 'default'
  enabled: true
  name: 'nova'
  # Generate with openssl rand -hex 10
  password: []
  project: 'service'
  role: 'admin'
  state: 'present'

# placement
openstack_compute_service_controller_placement:
  auth_type: 'password'
  auth_url: '{{ openstack_compute_service_controller_keystone_service_endpoint_url }}:35357/v3'
  password: "{{ openstack_compute_service_controller_placement_user_info['password'] }}"
  project_domain_name: "{{ openstack_compute_service_controller_placement_user_info['domain_id'] }}"
  project_name: "{{ openstack_compute_service_controller_placement_user_info['project'] }}"
  user_domain_name: "{{ openstack_compute_service_controller_placement_user_info['domain_id'] }}"
  username: "{{ openstack_compute_service_controller_placement_user_info['name'] }}"

openstack_compute_service_controller_placement_user_info:
  description: 'Placement user'
  domain_id: 'default'
  enabled: true
  name: 'placement'
  # Generate with openssl rand -hex 10
  password: []
  project: 'service'
  role: 'admin'
  state: 'present'

# RabbitMQ Connection Info
openstack_compute_service_controller_rabbit_host: 'localhost'
openstack_compute_service_controller_rabbit_pass: 'openstack'
openstack_compute_service_controller_rabbit_user: 'openstack'

openstack_compute_service_controller_services:
  - name: 'nova'
    service_type: 'compute'
    description: 'OpenStack Compute'
    state: 'present'
  - name: 'placement'
    service_type: 'placement'
    description: 'OpenStack Placement API'
    state: 'present'
```

## Dependencies

### Ansible Roles

The following [Ansible](https://www.ansible.com) roles are required as part of
this role.

-   [ansible-chrony](https://github.com/mrlesmithjr/ansible-chrony)
-   [ansible-config-interfaces](https://github.com/mrlesmithjr/ansible-config-interfaces)
-   [ansible-etc-hosts](https://github.com/mrlesmithjr/ansible-etc-hosts)
-   [ansible-memcached](https://github.com/mrlesmithjr/ansible-memcached)
-   [ansible-mysql](https://github.com/mrlesmithjr/ansible-mysql)
-   [ansible-openstack-base](https://github.com/mrlesmithjr/ansible-openstack-base)
-   [ansible-openstack-identity-service](https://github.com/mrlesmithjr/ansible-openstack-identity-service)
-   [ansible-openstack-image-service](https://github.com/mrlesmithjr/ansible-openstack-image-service)
-   [ansible-openstack-openrc](https://github.com/mrlesmithjr/ansible-openstack-openrc)
-   [ansible-rabbitmq](https://github.com/mrlesmithjr/ansible-rabbitmq)

The above roles can be installed using `ansible-galaxy` along with [requirements.yml](./requirements.yml):

```bash
ansible-galaxy install -r requirements.yml
```

## Example Playbook

[Example Playbook](./playbook.yml)

## License

MIT

## Author Information

Larry Smith Jr.

-   [@mrlesmithjr](https://www.twitter.com/mrlesmithjr)
-   [EverythingShouldBeVirtual](http://www.everythingshouldbevirtual.com)
-   mrlesmithjr [at] gmail.com