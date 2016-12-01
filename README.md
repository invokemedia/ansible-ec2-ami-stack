Ansible EC2 AMI Stack
======================

> Ansible script to create an entire EC2 HTTP stack in AWS using an existing AMI

This role is meant to be run locally. So see the [example playbook](#example-playbook) for how to do that.

Progress
--------

**WARNING:** This role is a work in progress.

The following currently works:

* create VPC
* add subnet to VPC
* add internet gateway to VPC
* setup route table for VPC and Gateway
* setup security group for HTTP, HTTPS, SSH
* provision EC2 server with an AMI (default [Invoke AMI](https://github.com/invokemedia/settler-ami-provision))

The following _is not_ working:

* RDS setup
* S3 setup

Installation
------------

* `mkdir -p server-setup/roles`
* `cd server-setup`
* `touch playbook.yml`
* `git clone https://github.com/invokemedia/ansible-ec2-ami-stack roles/invokemedia.ec2-ami-stack`

Role Variables
--------------

```yaml
# the name of the application. used in tags
app_name: Ansible
# which profile to use in boto
aws_boto_profile: default
# the AMI to use
aws_ami: ami-abad0ecb
# the key-pair to inject into the instance
aws_key_name: user-keyname
# instance type
aws_instance_type: t2.micro
# the region for all the resources
aws_region: us-west-2
# an output of the results of the task
ec2_vars_output: ./ec2-vars.yml
```

Dependencies
------------

None.

Example Playbook
-------------------------

Here is how you would use the default setup setup.

```yaml
- hosts: localhost
  connection: local
  gather_facts: True
  vars:
    app_name: ProjectName STAGE
  roles:
    - { role: invokemedia.ec2-ami-stack }
```

Then use `ansible-playbook playbook.yml -i 'localhost,'` to play.

This role will also create a `ec2-vars.yml` file in the root of the role. This is used so you can know what the results (ids, names, locations, ips, dns names, etc.) of all the resources are.

License
-------

MIT

Author Information
------------------

* [Invoke Media](http://www.invokemedia.com/)
* <webmaster@invokemedia.com>

References
----------

None.
