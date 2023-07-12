# Ansible-simple-project
Introduction: 
- Ansible is an open-source automation tool used for configuration management,
application deployment, and orchestration.
- It follows a declarative approach, where you define the desired state of the
system rather than scripting specific commands.
- Ansible uses SSH for communication with target systems, making it agentless
and easy to set up.

Inventory: The inventory file lists the target hosts on which Ansible will run
tasks. It can be a static file or generated dynamically.

Playbooks: Playbooks are YAML files that define a set of tasks and
configurations to be executed on target hosts.

Tasks: Tasks are the individual units of work in Ansible. They represent actions
to be performed on target hosts.

Modules: Ansible provides a wide range of modules for various tasks, such as
package installation, file manipulation, service management, etc.

Roles: Roles are reusable units of playbooks. They encapsulate related tasks,
handlers, variables, and files into a directory structure.
