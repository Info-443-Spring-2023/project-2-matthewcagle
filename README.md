[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/ZL2e6lYH)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-718a45dd9cf7e7f842a935f5ebbe5719a5e09af4491e668f4dbf3b35d5cca122.svg)](https://classroom.github.com/online_ide?assignment_repo_id=11076525&assignment_repo_type=AssignmentRepo)

# Context & Background  

## Software Description:
Ansible is a cross-platform tool for resource provisioning automation that can be used in combination with DevOps to make complex tasks easier to manage.A few of the tasks it can automate includes installing software, provisioning infastructure, improving security and complience and sharing automation.

## Software Creators:
Created by Michael Dehann and it was later acquired by Red Hat in 2015. Red Hat continues to maintain sensible and is a fairly mid sized with approximately 19,00 employees. 

# Development View
Ansible playbook execution overview:
The explanation below is an example of Ansible's functions and illustrates how the components we've chosen to analyze work together. Note that these are not the only modules and packages involved in the process, rather a high-level of abstraction.
Prior to playbook execution occurring, Ansible executes the `setup` module from `ansible.module_utils.facts` on each target host. This provides data about the hosts including operating system details, network interfaces, disk information, and installed packages. During playbook execution, gathered facts are accessed from stored memory.
To begin the execution, `ansible.playbook` reads and parse the playbook file interpreting the tasks, plays, and defined configurations. It identifies  the hosts and groups given in the playbook which are managed by `ansible.inventory`.
The `ansible.inventory` component deals with inventory management, containing information about target hosts and their grouping. It reads the inventory files and dynamic inventory sources and builds an inventory object. This object is passed back to the playbook execution process.
Task execution begins with the `ansible.executor` component which retrieves the list of tasks defined in the playbook and goes through them sequentially. The executor determines the target hosts and executes the associated modules. The proper modules are drawn from the `ansible.module_utils` package. 
While the execution is occurring, `ansible.config` component ensures that the desired configuration settings are applied. Configuration options can include connectivity, variable handling, file parsing, or playbook specific entities. 
All the while, `ansible.errors` is checking for errors and exceptions that may occur during playbook execution. This component provides error reporting, logging, and ways to address errors. If an error occurs, `ansible.executor` captures the error information and passes it to the `ansible.errors` component for appropriate handling.

### Table of High Level Components
| Component | Role & Relationship | Description |
| --------------------- | ------- | -------------------------- |
| Module_utils | Module Development. Used by the ansible playbook. | Provides utility modules which contain common functions and classes that can be used by module developers. Includes modules for handling command execution, working with files, and managing services. |
| Executer | Task execution. Depends on Module_utils.common. | Enables task execution and module invocation. Involves modules for task execution and management. |
| Module_utils.common | Module management | Common utility modules for working with test, URLs, locales, and other tasks. |
| Module_utils.facts | Module discovery. Used by ansible.playbook | Involves gathering facts and system information from remote hosts. |
| Playbook | Playbook development, depends on ansible.module_utils.facts | This package involves modules that work with Ansible playbooks. Allows for parsing, validating, and executing playbooks. |
| Config | Inventory management. | Provides modules for reading inventory files, manipulating host and group data, generating dynamic inventories, and playbook execution actions. |
| Errors | Error handling. Used by the Playbook. | Handles all errors raised by Ansible code. Returns the line in the file corresponding to the error and causes and suggested solutions for the error. |

# Applied Perspective

# Identify Style & Patterns Used

# Architectural Assessment 
## 1. Separation of Concerns
Ansible’s architecture adheres to this principle at a high level of abstraction through layer cohesion, as each of the modules are grouped together by their responsibility. This is exemplified under the `module_utils / facts ` package where data is collected to then be used in playbook execution. Under this are the following packages:

* `hardware`
* `network`
* `other`
* `system`
* `virtual`

Each package is then further broken up into modules based on different concerns such as the hardware package that deals with data related to hardware systems. The hardware system contains the following modules:

* `aix.py`
* `base.py`
* `darwin.py`
* `dragonfly.py`
* …

Ansible is broken up into distinct categories and maintains high cohesion so the the system is easier to maintain and improve. At a higher level Ansible is able to follow this principle but when analyzing at a code-level of abstraction it fails to adhere to this principle. In the AIXHardware class, it does have different methods for retrieving facts such as `get_cpu_facts`, `get_memory_facts`, and `get_dmi_facts`. But in the `populate` method it then combines the results from these methods into `hardware_facts.` To achieve separation of concerns would be better to further break up the code so that each is retrieving a specific type of hardware information. 


