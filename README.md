# Ansible Project: ansible-aci-model
A sample Ansible project to model and deploy payload on top of Cisco ACI fabrics.

This project provides an abstraction layer that is convenient to use. By providing your required configuration (a structured dataset) in your inventory this project will perform the needed actions to ensure that configuration is deployed on your ACI infrastructure.

Using this project you can easily set up demo environment, maintain a lab or use it as the basis for your in-house ACI infrastructure. It can help you understand how ACI works while prototyping and testing. No prior Ansible or ACI knowledge is required to get started.


## Requirements
This role requires the [cisco.aci](https://galaxy.ansible.com/cisco/aci) collection and the standard setup of Ansible.


## Installation

### Install the environment
You can install the working environment either by using the system-wide package manager or by using the Python virtual environment.

Example commands to install Ansible and required dependencies packages for RHEL9 or its derivatives:

      sudo dnf install -y ansible
      sudo dnf install -y python3-lxml python3-cryptography python3-pyOpenSSL python3-dateutil

Example commands to create a Python virtual environment and install Ansible and required dependencies:

      python3 -m venv .venv
      source .venv/bin/activate
      pip install -U pip
      pip install ansible
      pip install -r requirements.txt

### Install the project
You can install this project by cloning the GitHub repository:

    git clone https://github.com/joninsk/ansible-aci-model ansible-aci-model

### Install the dependencies
You can install the [cisco.aci](https://galaxy.ansible.com/cisco/aci) collection directly by using the following command:

    ansible-galaxy collection install cisco.aci

or from requirements file:

    ansible-galaxy collection install -r requirements.yml

### Install the aci filter plugin
In order to work with the provided ACI topology, a custom Jinja2 filter (*aci_listify*) is needed.
You need to configure your Ansible to find this Jinja2 filter. There are two ways to do this:

Configure Ansible so it looks for the custom aci filter plugin:

      ```ini
      filter_plugin = /home/user/ansible-aci-model/plugins/filter
      ```

Or copy the filter plugin [plugins/filter/aci.py](plugins/filter/aci.py) into your designated filter plugin directory.

Because of its general usefulness, we are looking into making this *aci_listify* filter more generic and make it part of the default Ansible filters.

### Ensure the playbook can find your cisco.aci collection
If you installed the collection from Galaxy, you should be fine to use the examples from GitHub:

    ansible-galaxy collection list cisco.aci

    # /usr/lib/python3.9/site-packages/ansible_collections
    Collection Version
    ---------- -------
    cisco.aci  2.1.0

    # /home/user/.ansible/collections/ansible_collections
    Collection Version
    ---------- -------
    cisco.aci  2.3.0
## Using the example playbook

### Configure your APIC host and credentials
Look inside the example inventory [hosts](hosts) and provide the needed information.
Only the first APIC is being use by the playbook, so you don't need more than one.

#### Access Credentials
Access credentials have to be defined in the inventory:

- apic_host
- apic_username (defaults to 'admin')
- apic_password

- apic_use_proxy (defaults to false)
- apic_validate_certs (defaults to true)

You can configure these as part of your host variables or group variables.

### Running the example playbook
Run the following command using installed Ansible:

    ansible-playbook main.yml -v

The first time it will deploy that configuration on your ACI infrastructure.

You can make modifications and run it again as often as you like to modify the existing configuration.


## Examples

### Example inventory
An example of a topology defined in [aci.yml](group_vars/fabric01/aci.yml) you can use with this model.

### Example playbook

```yaml
- name: Deploy topology using ACI model
  hosts: '*apic1'
  gather_facts: no
  tasks:
  - include_tasks: tasks/main.yml
```

## Notes
- This is a derivative of [datacenter/ansible-role-aci-model](https://github.com/datacenter/ansible-role-aci-model) role for demonstration of Ansible capabilities to configure the APIC fabrics.
- The Cisco Sandbox APIC is an online testing environment hosted by Cisco.
- Feel free to modify, extend and add additional functionality and share it on GitHub!


## License
GPLv3
