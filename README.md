# Consul

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/consul/status.svg)](https://drone.osshelp.ru/ansible/consul)

Role which installs Consul and generates needed JSON-config.

## Deploy example (do not copy blindly!)

```yaml
    - role: consul
      consul_version: "1.6.3"
      consul_params:
        encrypt: "secret_key_here"
        datacenter: "test-datacenter"
        node_name: "test-node"
        data_dir: "/var/lib/consul"
        retry_join: [ "netdata-master" ]
        pid_file: "/var/run/consul/consul.pid"
        enable_local_script_checks: true
```

Secret keys must be base64, or Consul will not start.

## About JSON generation

Configs are being generated from YML (variable `consul_params`). Consul has quite straight-forward configuration, so in most cases you will be able to describe with YML any parameters needed. For example, this:

```yaml
consul_params:
  enable_local_script_checks: true
  pid_file: "/var/run/consul/consul.pid"
  retry_join: [ "netdata-master" ]
```

will result in such JSON:

```json
{
    "enable_local_script_checks": true,
    "pid_file": "/var/run/consul/consul.pid",
    "retry_join": [
        "netdata-master"
    ]
}
```

## Cluster initialization

Not supported by the role, and there is no assurance that it will be implemented. You need to bootstrap manually.

## Services registration

Not supported yet, but will be added later. For now just put needed JSONs via additional tasks.

## Useful links

- [Official documentation](https://www.consul.io/docs/index.html)
- [Consul releases available](https://releases.hashicorp.com/consul/)
- [Our article with general information about Consul](https://rm.osshelp.ru/projects/support-servers/knowledgebase/articles/3364)
- [Our article, describing Consul setup and configuration](https://oss.help/kb3367)

## TODO

- Check the role in various builds
- Make custom services registration available
