# ServiceNow CMDB flavour

Placeholder. The intent is to treat the ServiceNow CMDB as the source of truth
and have AAP consume it, rather than maintaining a parallel record.

Scope to settle before any implementation:

- Which CI classes map to the records in `../git-cmdb/`.
- Whether AAP reads the CMDB through a dynamic inventory source or a lookup at
  job time.
- Reconciliation direction: does automation write discovered state back, and
  under what approval.
- Overlap with `ServiceNow-ITSM-Integration`, which owns incident, catalog and
  the existing CMDB reconciliation use case. That repo owns the ITSM side; this
  one owns inventory consumption. Avoid implementing the same thing twice.
