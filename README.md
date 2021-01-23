# Consul

| master |
| -------- |
| [![Build Status](https://drone.osshelp.ru/api/badges/ansible/consul/status.svg)](https://drone.osshelp.ru/ansible/consul) |

Role which installs Consul and generates needed JSON-configs.

## Deploy example (do not copy blindly!)

```yaml
    - role: consul
      consul_version: "1.6.3"
      consul_agent_params:
        encrypt: "secret_key_here"
        datacenter: "test-datacenter"
        node_name: "test-node"
        data_dir: "/var/lib/consul"
        retry_join: [ "netdata-master" ]
        pid_file: "/var/run/consul/consul.pid"
        enable_local_script_checks: true
      consul_configure_server: true
      consul_server_params:
        encrypt: "secret_key_here"
        server: true
        enable_syslog: true
        disable_remote_exec: true
        rejoin_after_leave: true
```

Pay attention to consul_configure_server variable. By default it is set to "false", so server.json will not be generated. Secret keys must be base64, or Consul will not start.

## About JSON generation

Configs are being generated from YML (variables consul_agent_params and consul_server_params). Consul has quite straight-forward configuration for agent/server, so in most cases you will be able to describe with YML any parameters needed. For example, this:

```yaml
      consul_agent_params:
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
