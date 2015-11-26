# Ansible Inventory from VMWare

Simple python script which uses PySphere to fill the Ansible inventory from VMWare vCenter.
Originally Cloned from: [Github repository](https://github.com/RaymiiOrg/ansible-vmware)  =
[Official Website](https://raymii.org/s/software/Ansible__Dynamic_Inventory_From_VMware_vCenter.html)

My fork has added an option to override the strict SSL Cert Validation:
https://www.python.org/dev/peps/pep-0476/ as well as command line option parsing
with argparse.

### Installation

Install PySphere:

	pip install -U pysphere

Edit the script's login data and VCenter server FQDN:

	server_fqdn = "vcenter.example.org"
	server_username = "jdoe"
	server_password = "secure_passw0rd"


### Test it:

Get all powered on VM's:

	python2 query.py --list

Output:

	{
	    "no_group": {
	        "hosts": [
	            "vm0031",
	            "vm0032",
	            [...]
	            "vm0999"
	        ]
	    },
	    "local": [
	        "127.0.0.1"
	    ]
	}

Get one VM:

	python2 query.py --host vm0032

Output:

	{
	    "admin": "sysadmin@example.org",
	    "source_database": "VMWare"
	}

Nonexistent VM:

	python2 query.py --host nonexist

Output:

	[Error]: [Object Not Found]: Could not find a VM named 'notexist'

Do a simple ansible ping:

	ANSIBLE_HOSTS="/home/remy/git/vmware-ansible/query.py" ansible all -m ping

Result:

	vm0032 | success >> {
	    "changed": false,
	    "ping": "pong"
	}

	vm0034 | success >> {
	    "changed": false,
	    "ping": "pong"
	}

### Usage

Either export the ANSIBLE_HOSTS variable to always use the VMWare inventory:

	export ANSIBLE_HOSTS="/home/remy/git/vmware-ansible/query.py"

Or preface each ansible command:

	ANSIBLE_HOSTS="/home/remy/git/vmware-ansible/query.py" ansible all -m apt -a "upgrade=safe"

### Notes

Tested with both vCenter 5.1 and 5.5, Python 2.  

Does not support grouping by datacenter or resource group

Thanks to [JP Mens](http://jpmens.net/2013/06/18/adapting-inventory-for-ansible/)'s article for the inspiration.
