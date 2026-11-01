# Generated-from-facts flavour

Placeholder. The intent is to render an inventory view from gathered facts
rather than from a declared record.

Scope to settle before any implementation:

- Fact collection and caching: where facts come from and how stale they may be.
- What this flavour is for — most likely drift detection against `../git-cmdb/`
  rather than a source of truth in its own right.
- Tooling choice. The original `ansible-cmdb` project is unmaintained; decide
  between it, a template over the fact cache, or AAP's own host facts.
