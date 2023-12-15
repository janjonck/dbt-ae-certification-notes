# Source Properties

All content is based on: [dbt docs (source properties)](https://docs.getdbt.com/reference/source-properties)

## Overview

Source properties can be declared in any ```properties.yml``` file in your ```models/``` directory (as defined by the model-paths config). Source properties are "special properties" in that you can't configure them in the ```dbt_project.yml``` file or using config() blocks. Refer to Configs and properties for more info.
You can name these files ```whatever_you_want.yml```, and nest them arbitrarily deeply in subfolders within the ```models/directory```. Below you can find an overview of the structure of a ```models/<filename>.yml``` file:



```yml
version: 2

sources:
  - name: <string> # required
    description: <markdown_string>
    database: <database_name>
    schema: <schema_name>
    loader: <string>
    loaded_at_field: <column_name>
    meta: {<dictionary>}
    tags: [<string>]
    
    # requires v1.1+
    config:
      <source_config>: <config_value>

    overrides: <string>

    freshness:
      warn_after:
        count: <positive_integer>
        period: minute | hour | day
      error_after:
        count: <positive_integer>
        period: minute | hour | day
      filter: <where-condition>

    quoting:
      database: true | false
      schema: true | false
      identifier: true | false

    tables:
      - name: <string> #required
        description: <markdown_string>
        meta: {<dictionary>}
        identifier: <table_name>
        loaded_at_field: <column_name>
        tests:
          - <test>
          - ... # declare additional tests
        tags: [<string>]
        freshness:
          warn_after:
            count: <positive_integer>
            period: minute | hour | day
          error_after:
            count: <positive_integer>
            period: minute | hour | day
          filter: <where-condition>

        quoting:
          database: true | false
          schema: true | false
          identifier: true | false
        external: {<dictionary>}
        columns:
          - name: <column_name> # required
            description: <markdown_string>
            meta: {<dictionary>}
            quote: true | false
            tests:
              - <test>
              - ... # declare additional tests
            tags: [<string>]
          - name: ... # declare properties of additional columns

      - name: ... # declare properties of additional source tables

  - name: ... # declare properties of additional sources
```

## Example of filled in ```models/<filename>.yml``` file:


```yml
version: 2

sources:
  - name: jaffle_shop
    database: raw
    schema: public
    loader: emr # informational only (free text)
    loaded_at_field: _loaded_at # configure for all sources

    # meta fields are rendered in auto-generated documentation
    meta:
      contains_pii: true
      owner: "@alice"

    # Add tags to this source
    tags:
      - ecom
      - pii

    quoting:
      database: false
      schema: false
      identifier: false

    tables:
      - name: orders
        identifier: Orders_
        loaded_at_field: updated_at # override source defaults
        columns:
          - name: id
            tests:
              - unique

          - name: price_in_usd
            tests:
              - not_null

      - name: customers
        quoting:
          identifier: true # override source defaults
        columns:
            tests:
              - unique
```
