# Consul

Install and configure a Consul agent.

The agent can act as a client or as a server if the host is placed in a group called `consul` in the inventory.

## Variables

* `consul_version`: The version of Consul to install.
* `consul_checksum`: The checksum of the tarball provided by Hashicorp.
* `consul_bind_address`: The address that should be bound to for internal cluster communications. This is an IP address that should be reachable by all other nodes in the cluster.
* `consul_advertise_address`: The advertise address is used to change the address that we advertise to other nodes in the cluster. By default, the -bind address is advertised.
* `consul_client_address`: The address to which Consul will bind client interfaces, including the HTTP, DNS, and RPC servers. By default, this is "127.0.0.1", allowing only loopback connections.
* `consul_datacenter`: The datacenter in which the agent is running.
* `consul_domain`: By default, Consul responds to DNS queries in the "consul." domain. This flag can be used to change that domain. All queries in this domain are assumed to be handled by Consul and will not be recursively resolved.
* `consul_retry_join`: Address of another agent to join upon starting up. This can be specified multiple times to specify multiple agents to join.
* `consul_ui_enabled`: Whether to enable the UI or not, default is false.

## Bootstrapping

All the consul agents are using the [`retry_join`](https://www.consul.io/docs/agent/options.html#retry_join) option to
know how to join the cluster.

For the initial seed, they all have the IP addresses of the others in their config and will try to reach each other
until all the servers specified in the `consul` group are available (specified by the
[`bootstrap_expect`](https://www.consul.io/docs/agent/options.html#bootstrap_expect) option).
When all the servers become available, they will do the election and client will be able to join the cluster too.