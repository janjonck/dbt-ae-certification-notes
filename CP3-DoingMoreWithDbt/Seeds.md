## Overview
Seeds are CSV files in your dbt project (typically in your `seeds` directory), that dbt can load into your data-warehouse using the `dbt seed` command.

Seeds can be referenced in downstream models the same way as referencing models — by using the `ref` function.

Because these CSV files are located in your dbt repository, they are version controlled and code reviewable. Seeds are best suited to static data which changes infrequently.

Good use-cases for seeds:
* A list of mappings of country codes to country names
* A list of test emails to exclude from analysis
* A list of employee account IDs

Poor use-cases of dbt seeds:
* Loading raw data that has been exported to CSVs
* Any kind of production data containing sensitive information. For example
personal identifiable information (PII) and passwords.


## Example
To load a seed file in your dbt project:
1. Add the file to your `seeds` directory, with a `.csv` file extension, e.g. `seeds/country_codes.csv`

```text
country_code,country_name
US,United States
CA,Canada
GB,United Kingdom
...
```


2. Run the `dbt seed` command — a new table will be created in your warehouse in your target schema, named `country_codes`
```
$ dbt seed

Found 2 models, 3 tests, 0 archives, 0 analyses, 53 macros, 0 operations, 1 seed file

14:46:15 | Concurrency: 1 threads (target='dev')
14:46:15 |
14:46:15 | 1 of 1 START seed file analytics.country_codes........................... [RUN]
14:46:15 | 1 of 1 OK loaded seed file analytics.country_codes....................... [INSERT 3 in 0.01s]
14:46:16 |
14:46:16 | Finished running 1 seed in 0.14s.

Completed successfully

Done. PASS=1 ERROR=0 SKIP=0 TOTAL=1
```

3. Refer to seeds in downstream models using the `ref` function.

```sql
-- This refers to the table created from seeds/country_codes.csv
select * from {{ ref('country_codes') }}
```

## Seed properties

Seed properties can be declared in `.yml` files under a `seed` key.

We recommend that you put them in the `seeds/` directory. You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within that directory.

```yml
version: 2

seeds:
  - name: <string>
    [description](/reference/resource-properties/description): <markdown_string>
    [docs](/reference/resource-configs/docs):
      show: true | false
    [config](/reference/resource-properties/config):
      [<seed_config>](/reference/seed-configs): <config_value>
    [tests](/reference/resource-properties/data-tests):
      - <test>
      - ... # declare additional tests
    columns:
      - name: <column name>
        [description](/reference/resource-properties/description): <markdown_string>
        [meta](/reference/resource-configs/meta): {<dictionary>}
        [quote](/reference/resource-properties/quote): true | false
        [tags](/reference/resource-configs/tags): [<string>]
        [tests](/reference/resource-properties/data-tests):
          - <test>
          - ... # declare additional tests

      - name: ... # declare properties of additional columns

  - name: ... # declare properties of additional seeds
```

## Configurationms

### Seed-specific configurations

The following resource-specific configurations are only available to Seeds:

`dbt_project.yml`

```yml
seeds:
  <resource-path>:
    +quote_columns: true | false
    +column_types: {column_name: datatype}
    +delimiter: <string>
```

`seeds/properties.yml`

```yml
version: 2

seeds:
  - name: [<seed-name>]
    config:
      quote_columns: true | false
      column_types: {column_name: datatype}
      delimiter: <string>
```

### General configurations

General seed configurations:

`dbt_project.yml`

```yml
seeds:
  <resource-path>:
    +enabled: true | false
    +tags: <string> | [<string>]
    +pre-hook: <sql-statement> | [<sql-statement>]
    +post-hook: <sql-statement> | [<sql-statement>]
    +database: <string>
    +schema: <string>
    +alias: <string>
    +persist_docs: <dict>
    +full_refresh: <boolean>
    +meta: {<dictionary>}
    +grants: {<dictionary>}
```

`seeds/properties.yml`

```yml
version: 2

seeds:
  - name: [<seed-name>]
    config:
      enabled: true | false
      tags: <string> | [<string>]
      pre-hook: <sql-statement> | [<sql-statement>]
      post-hook: <sql-statement> | [<sql-statement>]
      database: <string>
      schema: <string>
      alias: <string>
      persist_docs: <dict>
      full_refresh: <boolean>
      meta: {<dictionary>}
      grants: {<dictionary>}
```



