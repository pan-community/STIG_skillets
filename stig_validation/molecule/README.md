Overview
--------

Molecule is a testing framework specifically designed for Ansible roles. It has the ability to create and destory instaces specifically for testing, install dependencies, and run the role all while validating results are as expected. Molecule can be used to creat multiple scenarios. In the cas of this project there is a happy-path and a sad-path scenario. Each of these scenarios have an almost identical directory structure and operate using the same prerequisites and commands. The happy-path generates a config that will cause checks to pass while the sad-path generates a config that will cause checks to fail and the outputs for each are tested to validate the expected outcome.

Prerequisites
-------------

This framework has not fully been developed to include both create and delete functionality so existing instances must be used. Both the inventory and the host_vars must be filled out to match the test instances. The converge playbook must be adapted to work with the repo as explained below.

converge.yml
------------

The converge playbook prepares the instance for testing. The converge playbook creates a full configuration file using the jinja template, importing it to the device, and loading the config on the device. After those prerequisites complete the converge playbook runs the skillet files using the parent roles. The way these skillet files are run will have to be altered to fit the repo this testing framework is contained in. After each task completes successfully the desired outputs will be produced and those outputs can be tested.

verify.yml
----------

The verify playbook validates the outputs produced from the converge playbook. The verification is a two step process for each check. The first step is capturing the outputs from the converge playbook. In this case the capture step pulls the status produced in the checklist for a given check. The second step of the verification process is validating that the captured output matches what was expected. This validation is achieved by using the assert module native to Ansible.

Execution
------------

Setup the instance for testing by running the following command:
ansible-playbook converge.yml -i inventory/inventory -t <desired tags>

Validate the outputs by running the following command:
ansible-playbook verify.yml -i inventory/inventory -t <desired tags>
