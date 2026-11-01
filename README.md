# CMDB for AAP

An authoritative record of what exists in the demo estate — VM identities,
hostnames, roles, lifecycle, networks, interfaces, MAC addresses, address
assignments and AAP connection settings — and the ways Ansible Automation
Platform can consume it.

The point is not to build a CMDB product. It is to have one place that answers
"what is this host, what is it for, and how does AAP reach it", and to compare
how that question is best answered in a customer environment.

## Flavours

| Directory | Flavour | Idea |
|---|---|---|
| `git-cmdb/` | Git as the CMDB | Plain YAML records in version control. Review via pull request, history for free, no service to run. The simplest thing that works. |
| `servicenow-cmdb/` | ServiceNow CMDB | The enterprise answer. Reconcile records against the real CMDB and drive AAP inventory from it. Overlaps with the [ServiceNow-ITSM-Integration](https://github.com/kvegh/ServiceNow-ITSM-Integration) project, which owns the ITSM side. |
| `ansible-cmdb/` | Generated from facts | Render a browsable inventory from gathered facts. Discovery rather than declaration — useful to find drift against the other two. |

Each flavour answers the same questions with a different source of truth, so
they can be demonstrated side by side.

## Status

Early. `git-cmdb/` holds the first real record, carried over from the
`AAP-advanced-features` umbrella where it had no proper home. The other two
flavours are directories and intent so far. See [TODO.md](TODO.md).

## Secrets

Environment-specific values stay ansible-vault encrypted. No plaintext
hostnames, addresses, MACs or credentials in this repository.
