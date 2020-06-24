# Ansible Service Now examples


## Install Collection

Dependencies:

```bash
pip3 install pysnow
```

Collection:


```bash
ansible-galaxy collection install servicenow.servicenow
```

[Installing roles and collections from the same requirements.yml file](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-roles-and-collections-from-the-same-requirements-yml-file)
While both roles and collections can be specified in one requirements file, they need to be installed separately. The `ansible-galaxy role install -r requirements.yml` will only install roles and `ansible-galaxy collection install -r requirements.yml` -p ./ will only install collections.


## Running the examples

Export variables:

```bash
export SNOW_INSTANCE=<dev12345> # without '.service-now.com'
export SNOW_USER=<admin>
export SNOW_PASSWORD=<password>
```

Run any of the playbooks:

```python
⇨  ansible-playbook create-incident.yml 
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

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


## Links

- [Galaxy - Service Now](https://galaxy.ansible.com/servicenow/servicenow)
- [Ansible + ServiceNow Part 1: Opening and Closing Tickets](https://www.ansible.com/blog/ansible-servicenow-opening-and-closing-tickets)
- [Ansible + ServiceNow Part 2: Parsing facts from network devices using PyATS/Genie](https://www.ansible.com/blog/ansible-servicenow-part-2-parsing-facts-from-network-devices-using-pyats/genie)
- [Ansible + ServiceNow Part 3: Making outbound RESTful API calls to Red Hat Ansible Tower](https://www.ansible.com/blog/ansible-servicenow-howto-part-3-making-outbound-restful-api-calls-to-ansible-tower)
- [Governing Self-Service Cloud Provisioning](https://github.com/michaelford85/aws-deploy)