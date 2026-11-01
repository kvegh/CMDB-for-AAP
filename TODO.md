# TODO

Moved out of the `AAP-advanced-features` umbrella, which had no repository to
own this.

## Define the record

- [ ] Define the authoritative source for VM identities, hostnames, roles,
      lifecycle, hypervisors, NIC MACs and names, networks, addresses, and AAP
      connection settings.
- [ ] Distinguish permanent infrastructure with stable assignments from
      ephemeral test deployments. Discover or register new deployments and
      retire their records when removed; ephemeral VMs do not require permanent
      address reservations.
- [ ] Decide how AAP inventory, VM deployment inputs and network configuration
      consume this source.
- [ ] Keep sensitive and environment-specific values vault-encrypted or in a
      protected external system. Never commit plaintext credentials or
      infrastructure mappings.

## Reconcile with the hypervisor configuration

The hypervisor configuration repository already holds the *desired* state it
applies — libvirt networks and domains, bridges, subnets, DHCP. The record in
`git-cmdb/` is an *observed* snapshot of the same estate, so networks, bridges,
subnets, DHCP and MACs are duplicated between them.

- [ ] Decide which side owns network and domain definitions, and stop
      maintaining them twice.
- [ ] These fields in `git-cmdb/` have no equivalent in the hypervisor
      configuration and are the reason this record exists:
      `aap_connection_address`, `role`, `lifecycle`, `hostname_status`,
      `permanent_address_required`, plus the `follow_up` list.

## Per flavour

- [ ] `git-cmdb/` — the record is a point-in-time snapshot from 2026-10-06 with
      unresolved `follow_up` items. Confirm the open assignments, then decide
      whether anything reads it programmatically or it stays reference data.
- [ ] `servicenow-cmdb/` — settle scope against `ServiceNow-ITSM-Integration`
      before implementing, so the CMDB reconciliation use case is not built
      twice.
- [ ] `ansible-cmdb/` — choose the tooling; the original project is
      unmaintained.
