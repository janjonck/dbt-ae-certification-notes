Every [dbt project](/docs/build/projects) needs a `dbt_project.yml` file — this is how dbt knows a directory is a dbt project. It also contains important information that tells dbt how to operate your project.

<VersionBlock lastVersion="1.4">

By default, dbt will look for `dbt_project.yml` in your current working directory and its parents, but you can set a different directory using the `--project-dir` flag.

</VersionBlock>

<VersionBlock firstVersion="1.5">

By default, dbt will look for `dbt_project.yml` in your current working directory and its parents, but you can set a different directory using the `--project-dir` flag or the `DBT_PROJECT_DIR` environment variable.

Starting from dbt v1.5 and higher, you can specify your dbt Cloud project ID in the `dbt_project.yml` file using `project-id` under the `dbt-cloud` config. To find your project ID, check your dbt Cloud project URL, such as `https://cloud.getdbt.com/11/projects/123456`, where the project ID is `123456`.

</VersionBlock>

The following is a list of all available configurations in the `dbt_project.yml` file.

Example of a `dbt_project.yml` file:

```yml
name: string

config-version: 2
version: version

profile: profilename

model-paths: [directorypath]
seed-paths: [directorypath]
test-paths: [directorypath]
analysis-paths: [directorypath]
macro-paths: [directorypath]
snapshot-paths: [directorypath]
docs-paths: [directorypath]
asset-paths: [directorypath]

target-path: directorypath
log-path: directorypath
packages-install-path: directorypath

clean-targets: [directorypath]

query-comment: string

require-dbt-version: version-range | [version-range]

quoting:
  database: true | false
  schema: true | false
  identifier: true | false

models:
  <model-configs>

seeds:
  <seed-configs>

snapshots:
  <snapshot-configs>

sources:
  <source-configs>
  
tests:
  <test-configs>

vars:
  <variables>

on-run-start: sql-statement | [sql-statement]
on-run-end: sql-statement | [sql-statement]

dispatch:
  - macro_namespace: packagename
    search_order: [packagename]

restrict-access: true | false

```
