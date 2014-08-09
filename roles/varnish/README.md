## Standalone Varnish Deployment

- Requires Ansible 1.2 or newer
- Expects Ubuntu 12.02 Hosts

These playbooks deploy a standalone Varnish server.  A server is created in OpenStack,
and the appropriate Varnish software is installed and the Varnish configs located
here - https://github.com/DoSomething/ServerConfig are downloaded and added as the
Varnish config.
