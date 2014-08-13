Server Playbooks
========================

A collection of Ansible playbooks for use in setting up various servers.
Playbooks are a series of configuration steps and specifications for how the
server should be configured.  These playbooks require an OpenStack cloud environment
and the python nova client to run.

Information on installing novaclient:

http://docs.openstack.org/user-guide/content/install_clients.html

Executing the playbooks
---------------------------

### Setup Ansible

Playbooks require [Ansible][1] to execute them. It's really easy to setup, and
you can choose between running it on the same machine you're configuring, or a
remote machine. For a remote machine, all Ansible needs is the ability to establish
an SSH connection to it.

Generally, if you're looking for a quick time-saver for a one-time build of a
server, then you should set up Ansible and execute the playbook on the target
server. If you're a more serious server administrator who wants to maintain
clusters of machines, you should setup Ansible on a separate controller machine
or your personal machine.  These playbooks are written to be run from a controller
node.

You may refer to the setup guide on the Ansible homepage, however here are the
steps for setting it up on Ubuntu and immediately configuring it to use
localhost as the target server, the simplest configuration option:

    sudo aptitude -y install git python-jinja2 python-yaml python-paramiko python-software-properties
    add-apt-repository ppa:rquillo/ansible
    aptitude update
    aptitude install ansible
    echo "localhost" > /etc/ansible/hosts

You can now test by typing:

    ansible -c local -m ping all

You should see:

    localhost | success >> {
      "module": "ping",
      "ping": "pong
    }

### Run the play

The tasks for each of the plays are organized into roles.  Each playbook is designed
to spin up and configure new server or set of servers as defined by its respective
settings file.

    ansible-playbook mongoserver.yml --extra-vars createpassword=password

Common tasks like setting up a new dosomething user are defined in the common tasks
role.

Note: At the moment, Ansible doesn't have support for the --num-instances param that's
available in novaclient.  The workaround for this is to create a new host for each
new server you want to create in the os_api host group.  This is done automatically
for you by defining hostnames for each of the new nodes under the servers variable in:

roles/[role-name]/vars/[role-name]-settings.yml

Extras
---------------------------

To leverage dynamic inventory for nova:

1. Download https://github.com/blisteringherb/ansible-plugins
2. Make a backup of your current hosts file at /etc/ansible/hosts
3. Symlink the inventory/nova.py file to /etc/ansible/hosts and make sure it's executable
4. Symlink the inventory/nova.ini file to /etc/ansibile/nova.ini
5. Make the necessary changes to nova.ini and/or add the appropriate settings to
   your environment vars.
6. Run python /etc/ansible/hosts --list to check to make sure the dynamic inventory
   works and is getting all of the hosts from the tenant you specified.

[1]: http://ansible.github.com/ "Ansible"
