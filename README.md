# Consul

[![Build Status](https://drone.osshelp.io/api/badges/ansible/consul/status.svg)](https://drone.osshelp.io/ansible/consul)

Role which installs Consul and generates needed JSON-config.

## Deploy example (do not copy blindly!)

```yaml
    - role: consul
      consul_version: "1.11.1"
      consul_initial_setup: true
      consul_params:
        encrypt: "secret_key_here"
        datacenter: "test-datacenter"
        node_name: "test-node"
        data_dir: "/var/lib/consul"
        retry_join: [ "netdata-master" ]
        pid_file: "/var/run/consul/consul.pid"
        enable_local_script_checks: true
      consul_services:
        - service:
            id: netdata
            name: netdata
            tags: ['netdata']
            port: 19999
            checks:
              - id: netdata-health
                name: netdata-health
                http: http://localhost:19999/api/v1/info
                method: GET
                interval: 60s
                header:
                  accept: ['application/json']
        - service:
            id: sshd
            name: sshd
            tags: ['sshd']
            port: 22
            checks:
              - id: sshd-health
                name: sshd-health
                interval: 60s
                tcp: localhost:22
                timeout: 1s
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

## Services registration

You can describe any configuration with `consul_services` list (see example above), which is then converted to json configs. Keep in mind that this role can't delete old cfgs, after removing services from the list.

## Cluster initialization

Not supported by the role, and there is no assurance that it will be implemented. You need to bootstrap manually, or set `consul_params.bootstrap` to `true`.

## Versions available without URLs overwriting

- 1.14.0
- 1.13.3
- 1.12.6
- 1.11.11
- 1.11.1
- 1.10.12
- 1.9.17
- 1.8.19
- 1.7.14
- 1.6.3

## Useful links

- [Official documentation](https://www.consul.io/docs/index.html)
- [Official releases](https://releases.hashicorp.com/consul/)
- [Our article with general information about Consul](https://rm.osshelp.io/projects/support-servers/knowledgebase/articles/3364)
- [Our article, describing Consul setup and configuration](https://oss.help/kb3367)

## TODO

- ...

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
