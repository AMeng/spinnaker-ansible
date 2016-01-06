Spinnaker
=========

An ansible role for installing [Spinnaker](http://spinnaker.io).

Installation
------------

```
ansible-galaxy install AMeng.spinnaker
```

Role Variables
--------------

 * `spinnaker_install`: Boolean. Defaults to true. Should spinnaker be
   installed? If using the spinnaker AMI, you probably want to disable this.
 * `spinnaker_cassandra_local`: Boolean. Defaults to true. Will cassandra run
   locally?
 * `spinnaker_cassandra_version`: String. Defaults to "2.1.11"
 * `spinnaker_redis_local`: Boolean. Defaults to true. Will redis run locally?
 * `spinnaker_packer_version`: String. Defaults to "0.8.6"

Example Playbook
----------------
```yaml
- hosts: servers
  roles:
     - spinnaker
```

License
-------

MIT
