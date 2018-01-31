Ansible EC2 AMI Stack
======================

> Ansible script to create an entire EC2 HTTP stack in AWS using an existing AMI

This role is meant to be run locally. So see the [example playbook](#example-playbook) for how to do that.

Progress
--------

**WARNING:** This role is a work in progress.

The following currently works:

* Create VPC
* Add subnet to VPC
* Add internet gateway to VPC
* Setup route table for VPC and Gateway
* Setup security group for HTTP, HTTPS, SSH
* Provision EC2 server with an AMI (default [Invoke AMI](https://github.com/invokemedia/settler-ami-provision))

The following _is not_ working:

* Setup RDS
* Setup S3

Installation
------------

* `mkdir -p ansible/roles`
* `cd ansible`
* `touch playbook.yml`
* `git clone https://github.com/invokemedia/ansible-ec2-ami-stack roles/invokemedia.ec2-ami-stack`

Role Variables
--------------

```yaml
# Application details. Used to tag instances
app_env: stage
app_project: website
app_client: Invoke

# Which profile to use in boto
aws_boto_profile: default
# the AMI to use
aws_ami: ami-d4902bb4
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
    app_env: stage
    app_project: website
    app_client: Invoke
  roles:
    - { role: invokemedia.ec2-ami-stack }
```

Then use `ansible-playbook playbook.yml -i 'localhost,' --skip-tags "rds,s3"` to play.

This role will also create a `ec2-vars.yml` file in the root of the role. This is used so you can know what the results (ids, names, locations, ips, dns names, etc.) of all the resources are.

License
-------

MIT

Author Information
------------------

* [Invoke](http://www.invokedigital.co/)
* <webmaster@invokedigital.co>

References
----------

None.
