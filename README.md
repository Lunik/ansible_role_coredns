# Ansible Role: CoreDNS

[![MIT licensed][badge-license]][link-license]
[![Galaxy Role][badge-role]][link-galaxy]
[![Downloads][badge-downloads]][link-galaxy]

Installs [CoreDNS][coredns] on Linux and configure the service and template DNS zones.

## Requirements

None.

## Role Variables

| Variable                    | Type         | Description                                                                                   |
|:---------------------------:|:------------:|:----------------------------------------------------------------------------------------------|
| `coredns_version`           | string       | Version of CoreDNS to install                                                                 |
| `coredns_dns_port`          | number       | Listen port of CoreDNS service                                                                |
| `coredns_forwarders`        | list(string) | List of DNS server where to forward request if CoreDNS server don't have the answer           |
| `coredns_acls`              | list(object) | List of `acls` object defining who can/can't make DNS queries through the CoreDNS instance    |
| `coredns_zones`             | list(object) | Liste of `zone` object defining DNS zone where the CoreDNS server have authority              |
| `coredns_cache_prefetch`    | object       | Parameters to configure cache prefetch of the [CoreDNS cache plugin][coredns-cache-plugin]    |
| `coredns_cache_serve_stale` | object       | Parameters to configure cache serve stale of the [CoreDNS cache plugin][coredns-cache-plugin] |
| `coredns_ttl`               | object       | Parameters to configure cache TTL of the [CoreDNS cache plugin][coredns-cache-plugin]         |

### ACL

This section explain how to configure the [CoreDNS acl plugin][coredns-acl-plugin] using the `coredns_acls` variable.
A CoreDNS `acl` define who can/can't make DNS queries through the CoreDNS instance.

Each `acl` is defined with the following attributes :

| Attribute | Type   | Description |
|:---------:|:------:|:------------|
| `cidr`    | string | An IP CIDR (@IP or range) |
| `action`  | string | Action to apply when a client from that CIDR make a query |

### Zone

This section explain how to configure zones using the [CoreDNS file plugin][coredns-file-plugin] using the `coredns_zones` variable.
Each zone is defined with the following attributes :

| Attribute  | Type   | Description                                      |
|:----------:|:------:|:-------------------------------------------------|
| `name`     | string | The name of the DNS zone                         |
| `zone`     | string | Hostname of the zone (`example.org` for example) |
| `file`     | string | The name of the zone database file               |
| `template` | string | Path of the zone database template file          |

## Dependencies

None.

## Example Playbook

    - hosts: localhost
      vars:
        coredns_forwarders:
          - 9.9.9.9
        coredns_acls:
          - cidr: 192.168.0.0/24
            action: allow
        coredns_zones:
          - name: my-zone
            zone: my-zone.fr
            file: db.my-zone.fr
            template: templates/dbs/my-zone.fr
      roles:
        - lunik.coredns

## License

[MIT][link-license]

## Author Information

This role was created in 2019 by [Lunik (Guillaume MARTINEZ)][author-website].

#### Maintainer(s)

- [Lunik (Guillaume MARTINEZ)](https://github.com/Lunik)

[author-website]: https://lunik.tiwabbit.fr/
[badge-downloads]: https://img.shields.io/ansible/role/d/56142.svg
[badge-license]: https://img.shields.io/github/license/Lunik/ansible_role_coredns.svg
[badge-role]: https://img.shields.io/ansible/role/56142.svg
[link-galaxy]: https://galaxy.ansible.com/lunik/coredns
[link-license]: https://raw.githubusercontent.com/Lunik/ansible_role_coredns/master/LICENSE
[coredns]: https://coredns.io
[coredns-acl-plugin]: https://coredns.io/plugins/acl/
[coredns-file-plugin]: https://coredns.io/plugins/file/
[coredns-cache-plugin]: https://coredns.io/plugins/cache/
