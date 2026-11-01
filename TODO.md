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

Both are declarative and they overlap, so the boundary needs to be explicit.
A first cut:

| Concern | Lives in | Example |
|---|---|---|
| Which hosts exist and how to reach them | CMDB | identities, roles, lifecycle, interfaces, MACs, addresses, AAP connection settings |
| What the platform itself is configured with | CaC | job templates, projects, credentials, credential types, organizations |
| How a change is actually performed | Deployment playbooks | create the VM, configure the network, install the product |

So the deployment playbooks are the consumers: they take *what* from the CMDB
and *how* from their own content, while CaC defines the AAP objects that launch
them. The part of CaC that is genuinely CMDB-derived is the inventory — hosts,
groups and per-host variables — which should be generated from the record
rather than maintained separately.

- [ ] Confirm or correct that boundary, then write it down somewhere the other
      projects can reference.
- [ ] Generate AAP inventory content from the CMDB instead of hand-maintaining
      it, and decide whether that is a sync job or a dynamic inventory source.
- [ ] Identify CaC variables that are really host facts and should move to the
      CMDB.

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
