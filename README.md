# Ansible Service Now example

These playbooks are meant to run as a Workflow to demonstrate the use of [Cisco IOS](https://cloud.redhat.com/ansible/automation-hub/cisco/ios) and [Service Now](https://galaxy.ansible.com/servicenow/servicenow) collections from [Galaxy](https://galaxy.ansible.com) and the Red Hat [Automation Hub](https://cloud.redhat.com/ansible/automation-hub). Each Playbook can run in a different Python virtual enviroment (`venv`).

![Workflow][1]

This workflow parses data from a Cisco IOS device to create and update an incident. The output should look like this in [Service Now](https://developer.servicenow.com/dev.do):

![ServiceNow][2]

## Dependencies:

### Python libraries

```bash
pip3 install pysnow pyats[library]
```

### Collections

```bash
ansible-galaxy collection install servicenow.servicenow
ansible-galaxy collection install cisco.ios
```

### Roles

```bash
ansible-galaxy role install -r roles/requirements.yml
```

While both roles and collections can be specified in one requirements file, they need to be installed separately. The `ansible-galaxy role install -r requirements.yml` will only install roles and `ansible-galaxy collection install -r requirements.yml -p ./` will only install collections. See [Installing roles and collections from the same requirements.yml file](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-roles-and-collections-from-the-same-requirements-yml-file).


## Running the examples locally for testing

You can pass extra vars `snow_instance`, `snow_user`, and `snow_password` or export environment variables:

```bash
export SNOW_INSTANCE=<dev12345> # without '.service-now.com'
export SN_USERNAME=<admin>
export SN_PASSWORD=<password>
```

Run any of the playbooks:

```python
⇨  ansible-playbook create-incident.yml 

PLAY [Using ServiceNow Collection] ******************************************************************************************************************************************

TASK [Create an incident] ***************************************************************************************************************************************************
changed: [localhost]

TASK [debug] ****************************************************************************************************************************************************************
ok: [localhost] => {
    "new_incident.record.number": "INC0010002"
}
```

To run the rest of them, you need to export the incident number you got.

```yaml
export SNOW_NUMBER=INC0010002
```

## Inventory

First, you need to export `SN_INSTANCE`, `SN_USERNAME`, and `SN_PASSWORD`. These identify your [Service Now](https://developer.servicenow.com/dev.do) instance and credentials. For example:

```bash
export SN_INSTANCE=<dev12345.service-now.com> # with '.service-now.com'
export SN_USERNAME=<admin>
export SN_PASSWORD=<password>
```

1. You can grab the display your full inventory

    ```json
    ⇨  ansible-inventory -i inventory-snow.yml --list
    {
        "_meta": {
            "hostvars": {
                "DataNoc_gw01_lanset_net": {
                    "sn_fqdn": "",
                    "sn_ip_address": "208.187.161.1",
                    "sn_name": "DataNoc.gw01.lanset.net",
                    "sn_sys_class_name": "Network Gear"
                },
                "IP_Router_1": {
                    "sn_fqdn": "",
                    "sn_ip_address": "10.0.0.1",
                    "sn_name": "IP-Router-1",
                    "sn_sys_class_name": "IP Router"
                },
                ...
            }
        },
        "all": {
            "children": [
                "group_IP_Router",
                "group_IP_Switch",
                "group_Network_Gear",
                "ungrouped"
            ]
        },
        "group_IP_Router": {
            "hosts": [
                "IP_Router_1",
                "IP_Router_2",
                "IP_Router_3"
            ]
        },
        "group_IP_Switch": {
            "hosts": [
                "IP_Switch_1"
            ]
        },
        "group_Network_Gear": {
            "hosts": [
                "DataNoc_gw01_lanset_net",
                "San_Diego_Gateway",
                "nc6500_a01",
                "ny8500_nbxs08",
                "ny8500_nbxs09"
            ]
        }
    }

    ```

2. Or see it as a graph based on the `keyed_groups`

    ```bash
    ⇨  ansible-inventory -i inventory-snow.yml --graph
    @all:
    |--@group_IP_Router:
    |  |--IP_Router_1
    |  |--IP_Router_2
    |  |--IP_Router_3
    |--@group_IP_Switch:
    |  |--IP_Switch_1
    |--@group_Network_Gear:
    |  |--DataNoc_gw01_lanset_net
    |  |--San_Diego_Gateway
    |  |--nc6500_a01
    |  |--ny8500_nbxs08
    |  |--ny8500_nbxs09
    |--@ungrouped:
    ```

In Tower, you will want to import this on Inventory sources. Have `ANSIBLE_INVENTORY_ENABLED: auto` as well as the other required variables in the `ENVIRONMENT VARIABLES` section.

![ServiceNow][3]

Last, but not least, you can add hosts to your inventory with `snow_record`. Take a look at this [example](add-to-inventory.yml)

## Links

- [Galaxy - Service Now](https://galaxy.ansible.com/servicenow/servicenow)
- [Ansible + ServiceNow Part 1: Opening and Closing Tickets](https://www.ansible.com/blog/ansible-servicenow-opening-and-closing-tickets)
- [Ansible + ServiceNow Part 2: Parsing facts from network devices using PyATS/Genie](https://www.ansible.com/blog/ansible-servicenow-part-2-parsing-facts-from-network-devices-using-pyats/genie)
- [Ansible + ServiceNow Part 3: Making outbound RESTful API calls to Red Hat Ansible Tower](https://www.ansible.com/blog/ansible-servicenow-howto-part-3-making-outbound-restful-api-calls-to-ansible-tower)
- [Governing Self-Service Cloud Provisioning](https://github.com/michaelford85/aws-deploy)
- [Using inventory plugins](https://docs.ansible.com/ansible/latest/plugins/inventory.html#using-inventory-plugins)
- [Jinja2 to construct vars and groups based on existing inventory](https://docs.ansible.com/ansible/latest/plugins/inventory/constructed.html)


[1]: images/snow_workflow.png
[2]: images/snow_view.png
[3]: images/snow_inventory_tower.png