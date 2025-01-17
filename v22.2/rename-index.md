---
title: ALTER INDEX ... RENAME TO
summary: The ALTER INDEX ... RENAME TO statement changes the name of an index for a table.
toc: true
docs_area: reference.sql
---

`RENAME TO` is a subcommand of [`ALTER INDEX`](alter-index.html) that changes the name of an index for a table.

{{site.data.alerts.callout_info}}
It is not possible to rename an index referenced by a view. For more details, see [View Dependencies](views.html#view-dependencies).
{{site.data.alerts.end}}

{% include {{ page.version.version }}/misc/schema-change-stmt-note.md %}

{% include {{ page.version.version }}/misc/schema-change-view-job.md %}

## Synopsis

<div>
{% remote_include https://raw.githubusercontent.com/cockroachdb/generated-diagrams/{{ page.release_info.crdb_branch_name }}/grammar_svg/rename_index.html %}
</div>

## Required privileges

The user must have the `CREATE` [privilege](security-reference/authorization.html#managing-privileges) on the table.

## Parameters

 Parameter | Description
-----------|-------------
 `IF EXISTS` | Rename the index only if an index `current_name` exists; if one does not exist, do not return an error.
 `table_name` | The name of the table with the index you want to use
 `index_name` | The current name of the index
 `name` | The [`name`](sql-grammar.html#name) you want to use for the index, which must be unique to its table and follow these [identifier rules](keywords-and-identifiers.html#identifiers).

## Example

{% include {{ page.version.version }}/sql/rename-index.md %}

## See also

- [Indexes](indexes.html)
- [`CREATE INDEX`](create-index.html)
- [`RENAME COLUMN`](rename-column.html)
- [`RENAME DATABASE`](rename-database.html)
- [`RENAME TABLE`](rename-table.html)
- [`SHOW JOBS`](show-jobs.html)
- [Online Schema Changes](online-schema-changes.html)
