# Consul

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/consul/status.svg)](https://drone.osshelp.ru/ansible/consul)

Role which installs Consul and generates needed JSON-config.

## Deploy example (do not copy blindly!)

```yaml
    - role: consul
      consul_version: "1.6.3"
      consul_initial_setup: true
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

## Advertising to existing interface

Could be done with parameters:

```yaml
consul_advertise_iface:
  enabled: true
  iface: eth0
```

Where `eth0` must be replaced with proper interface name.

## Controling UI path

Could be made by overriding `consul_ui_content_path` param:

```yaml
consul_ui_content_path: /ui/consul
```

By default, the path is `/ui/`, for example `http://localhost:8500/ui/`. Only alphanumerics, `-`, and `_` are allowed in a custom path. `/v1/` is not allowed as it would overwrite the API endpoint.

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

- Custom services registration
- Check the role in various builds
