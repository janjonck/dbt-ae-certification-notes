# Getting started with hooks and operations

Ref: [dbt docs](https://docs.getdbt.com/docs/build/hooks-operations)

Effective database administration sometimes requires additional SQL statements to be run, for example:
- Creating UDFs
- Managing row- or column-level permissions
- Vacuuming tables on Redshift
- Creating partitions in Redshift Spectrum external tables
- Resuming/pausing/resizing warehouses in Snowflake
- Refreshing a pipe in Snowflake
- Create a share on Snowflake
- Cloning a database on Snowflake

dbt provides hooks and operations so you can version control and execute these statements as part of your dbt project.

## About hooks

Hooks are snippets of SQL that are executed at different times:
  * `pre-hook`: executed _before_ a model, seed or snapshot is built.
  * `post-hook`: executed _after_ a model, seed or snapshot is built.
  * `on-run-start`: executed at the _start_ of `dbt build`, `dbt compile`, `dbt docs generate`, `dbt run`, `dbt seed`, `dbt snapshot`, or `dbt test`.
  * `on-run-end`: executed at the _end_ of `dbt build`, `dbt compile`, `dbt docs generate`, `dbt run`, `dbt seed`, `dbt snapshot`, or `dbt test`.

Hooks are a more-advanced capability that enable you to run custom SQL, and leverage database-specific actions, beyond what dbt makes available out-of-the-box with standard materializations and configurations.

If (and only if) you can't leverage the `grants` resource-config, you can use `post-hook` to perform more advanced workflows:

* Need to apply `grants` in a more complex way, which the dbt Core v1.2 `grants` config does not (yet) support.
* Need to perform post-processing that dbt does not support out-of-the-box. For example, `analyze table`, `alter table set property`, `alter table ... add row access policy`, etc.

### Examples using hooks

You can use hooks to trigger actions at certain times when running an operation or building a model, seed, or snapshot.

For more information about when hooks can be triggered, see reference sections for `on-run-start` and `on-run-end` hooks and `pre-hook`s and `post-hook`s.

You can use hooks to provide database-specific functionality not available out-of-the-box with dbt. For example, you can use a `config` block to run an `ALTER TABLE` statement right after building an individual model using a `post-hook`:

```sql
{{ config(
    post_hook=[
      "alter table {{ this }} ..."
    ]
) }}
```
