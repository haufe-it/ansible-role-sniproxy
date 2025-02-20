# sniproxy

Install and configure sniproxy on Debian hosts

## Requirements

OS: Debian or derivative

## Role variables

| Variable                     | Description
|-                             |-
`sniproxy_resolver_nameserver` | IP address of nameserver to use
`sniproxy_resolver_mode`       | Choose one of `ipv4_first`, `ipv6_first`, `ipv4_only`, `ipv6_only`
`sniproxy_listeners`           | Array of listeners, see below
`sniproxy_tables`              | Array of tables, see below

### Listeners

Each listener consists of a map of the following items:

| Variable   | Description 
|-           |-
| `socket`   | Listening socket specification like `80` or `[::]:443`
| `protocol` | One of `http` or `tls`
| `table`    | Lookup table to use

### Tables

Each table consists of a map of the following items:

| Variable  | Description 
|-          |-
| `name`    | Name for referencing the table from listeners
| `entries` | Array of table entries, see below

### Table entries

Each table entry consists of a map of the following items:

| Variable | Description 
|-         |- 
| `map`    | Regex to match incoming requests to
| `to`     | IP, optionally with port, or unix socket, or literal *
| `flags`  | `proxy_protocol` to prepend HAProxy headers, or absent


## Example configuration

```
sniproxy_resolver_nameserver: 53.53.53.53
sniproxy_resolver_mode: ipv6_only
sniproxy_listeners:
  - socket: 443
    protocol: tls
    table: https_hosts
sniproxy_tables:
  - name: https_hosts
    entries:
      - map: ^.*\.example\.com$
        to: "*"
```
