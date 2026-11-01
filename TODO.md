# TODO

Moved out of the `AAP-advanced-features` umbrella, which had no repository to
own this.

## The goal

Any change to any platform is declared in the CMDB first, then carried out by
automation through AAP. Nothing is created, renamed, re-addressed or retired by
hand on a hypervisor or a guest. The record is the intent; the job is the
mechanism; the running estate is the result. Drift is then a question that can
actually be asked, because there is something to compare against.

- [ ] Establish the order of operations: declare in the CMDB, review the change,
      then run the automation that applies it. Decide what enforces this — an
      approval on the record, a job that refuses unknown hosts, or both.
- [ ] Decide what happens to a change made outside this path. Detect it, report
      it, reconcile it back into the record, or revert it.

## Relationship to configuration-as-code

The CMDB is not only a host list. Applications and services are configuration
items too: AAP itself, its version, which hosts it runs on, its components and
endpoints, and its relationships to the systems it manages and integrates with.
The same applies to the other platforms in the estate.

So the boundary is a layer boundary, not a subject boundary. The CMDB declares
*that a thing exists, where, in which version, and how it relates to everything
else*. Configuration-as-code implements *the inside of that thing*, and does so
downstream of the record rather than beside it.

| Layer | Owns | Example |
|---|---|---|
| CMDB | Existence, placement, version, endpoints, relationships, lifecycle | AAP 2.7 runs on these hosts, serves this URL, manages these endnodes, integrates with ServiceNow and Zabbix |
| CaC | The internal configuration of a declared item | AAP's organizations, projects, job templates, credentials, inventories |
| Deployment playbooks | The act of making reality match | install AAP, create the VM, configure the network |

Which makes CaC a consumer of the record in two ways, not one: the inventory it
manages is CMDB-derived, and so are the facts about the platform it configures —
hostnames, URLs, versions, integration endpoints.

- [ ] Model applications and services as configuration items alongside hosts,
      with relationships between them, not just a flat host list.
- [ ] Decide the CI classes needed — host, application, service, network,
      integration — and keep that list small enough to maintain.
- [ ] Generate AAP inventory content from the CMDB rather than hand-maintaining
      it; decide whether that is a sync job or a dynamic inventory source.
- [ ] Identify CaC variables that are really CMDB facts — platform hostnames,
      URLs, versions, integration endpoints — and source them from the record.
- [ ] Decide what stays genuinely CaC-only: object definitions that express
      intent about behaviour rather than facts about the estate.

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
