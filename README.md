Spinnaker
=========

An ansible role for installing and configuring [Spinnaker](http://spinnaker.io).

Installation
------------

```
ansible-galaxy install AMeng.spinnaker
```

Role Variables
--------------

**Installation:**

 * `spinnaker_install`: Boolean. Defaults to true. Should spinnaker be
   installed? If using the spinnaker AMI, you probably want to disable this.
 * `spinnaker_backup`: Boolean. Defaults to false. Should a cron job be added
   to backup cassandra? Setting this true requires that you define
   `spinnaker_backup_bucket`.
 * `spinnaker_backup_bucket`: String. The S3 bucket to store cassandra backups.
   Include a trailing slash if using folder names.
 * `spinnaker_cassandra_local`: Boolean. Defaults to true. Will cassandra run
   locally?
 * `spinnaker_cassandra_version`: String. Defaults to "2.1.11"
 * `spinnaker_redis_local`: Boolean. Defaults to true. Will redis run locally?
 * `spinnaker_packer_version`: String. Defaults to "0.8.6"

**Configuration:**

* `spinnaker_environment`: Dictionary. Environment variables for the spinnaker
  process. See example playbooks below or check the Spinnaker
  [GitHub Repo](https://github.com/spinnaker/spinnaker/blob/master/etc/default/spinnaker)
  for more examples.

These variables are used to populate Spinnaker's YAML configuration files. See
the example playbooks below for more information. The YAML under each variable
will be copied to the following files:

|  Variable                    | File                  |
|------------------------------|-----------------------|
| spinnaker_config             | spinnaker-local.yml   |
| spinnaker_config_clouddriver | clouddriver-local.yml |
| spinnaker_config_echo        | echo-local.yml        |
| spinnaker_config_gate        | gate-local.yml        |
| spinnaker_config_igor        | igor-local.yml        |
| spinnaker_config_orca        | orca-local.yml        |
| spinnaker_config_rush        | rush-local.yml        |


Example Playbooks
-----------------

**Default configuration**:
```yaml
- hosts: servers
  roles:
    - AMeng.spinnaker
```

**Basic configuration**:
```yaml
- hosts: servers
  roles:
    - AMeng.spinnaker
  vars:
    spinnaker_backup: True
    spinnaker_backup_bucket: "MyBucket"
    spinnaker_environment:
      spinnaker_aws_enabled: "true"
    spinnaker_config:
      providers:
        aws:
          enabled: true
          defaultRegion: us-east-1
          defaultIAMRole: spinnaker
```

**Advanced configuration**:
```yaml
- hosts: servers
  roles:
    - AMeng.spinnaker
  vars:
    spinnaker_backup: True
    spinnaker_backup_bucket: "MyBucket/MyFolder/"
    spinnaker_environment:
      spinnaker_aws_enabled: "true"
      spinnaker_aws_default_region: us-east-1
      timezone: America/Denver
      aws_vpc_id: vpc-12345678
      aws_subnet_id: subnet-98765432
    spinnaker_config:
      providers:
        aws:
          enabled: true
          defaultRegion: us-east-1
          defaultIAMRole: spinnaker
    spinnaker_config_clouddriver:
      aws:
        accounts:
          - name: production
            accountId: 111111111111
            discovery: http://eureka.production.example.com
          - name: staging
            accountId: 222222222222
            discovery: http://eureka.staging.example.com
    spinnaker_config_gate:
      saml:
        enabled: true
        requireAuthentication: true
        url: https://example.okta.com/app/spinnaker/xxxxxxxxxxxxxxxxxxxx/sso/saml
        certificate: /path/to/certificate
        issuerId: http://www.okta.com/xxxxxxxxxxxxxxxxxxxx
        keyStore: /path/to/keystore
        keyStoreType: JKS
        keyStorePassword: password
        keyStoreAliasName: okta
```

Notes
-----

This playbook is only tested with AWS. While spinnaker supports other clouds,
this playbook has not been tested with them. Contributions for other providers
will be gladly accepted.

License
-------

MIT
