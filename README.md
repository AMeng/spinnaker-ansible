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

**Installation:**

 * `spinnaker_install`: Boolean. Defaults to true. Should spinnaker be
   installed? If using the spinnaker AMI, you probably want to disable this.
 * `spinnaker_cassandra_local`: Boolean. Defaults to true. Will cassandra run
   locally?
 * `spinnaker_cassandra_version`: String. Defaults to "2.1.11"
 * `spinnaker_redis_local`: Boolean. Defaults to true. Will redis run locally?
 * `spinnaker_packer_version`: String. Defaults to "0.8.6"

**Configuration:**

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
    - spinnaker
```

**Basic configuration**:
```yaml
- hosts: servers
  roles:
    - spinnaker
  vars:
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
    - spinnaker
  vars:
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

License
-------

MIT
