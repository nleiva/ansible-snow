# Ansible Service Now example

These playbooks are meant to run as a Workflow to demonstrate the use of Cisco IOS and Service Now collections from Galaxy and the Automation Hub. Each Playbook can run in a different Python virtual enviroment (`venv`).

![Workflow][1]


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

While both roles and collections can be specified in one requirements file, they need to be installed separately. The `ansible-galaxy role install -r requirements.yml` will only install roles and `ansible-galaxy collection install -r requirements.yml` -p ./ will only install collections. See [Installing roles and collections from the same requirements.yml file](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-roles-and-collections-from-the-same-requirements-yml-file).


## Running the examples locally for testing

You can pass extra vars `snow_instance`, `snow_user`, and `snow_password` or export environment variables:

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


[1]: images/snow_workflow.png