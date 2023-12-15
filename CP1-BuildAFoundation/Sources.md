## Using sources
Ref: [dbt docs](https://docs.getdbt.com/docs/build/sources)

Sources make it possible to name and describe the data loaded into your warehouse by your Extract and Load tools. By declaring these tables as sources in dbt, you can then
- select from source tables in your models using the `{{ source() }}` function, helping define the lineage of your data
- test your assumptions about your source data
- calculate the freshness of your source data

### Declaring a source

Sources are defined in `.yml` files nested under a `sources:` key.

```yaml
version: 2

sources:
  - name: jaffle_shop
    database: raw  
    schema: jaffle_shop  
    tables:
      - name: orders
      - name: customers

  - name: stripe
    tables:
      - name: payments
```

*By default, `schema` will be the same as `name`. Add `schema` only if you want to use a source name that differs from the existing schema.

If you're not already familiar with these files, be sure to check out the documentation on schema.yml files.

### Selecting from a source

Once a source has been defined, it can be referenced from a model using the `{{ source()}}` function.

```sql
select
  ...

from {{ source('jaffle_shop', 'orders') }}

left join {{ source('jaffle_shop', 'customers') }} using (customer_id)

```

dbt will compile this to the full table name:


```sql

select
  ...

from raw.jaffle_shop.orders

left join raw.jaffle_shop.customers using (customer_id)

```

Using the `{{ source () }}` function also creates a dependency between the model and the source table.

### Testing and documenting sources
You can also:
- Add data tests to sources
- Add descriptions to sources, that get rendered as part of your documentation site

```yaml
version: 2

sources:
  - name: jaffle_shop
    description: This is a replica of the Postgres database used by our app
    tables:
      - name: orders
        description: >
          One record per order. Includes cancelled and deleted orders.
        columns:
          - name: id
            description: Primary key of the orders table
            tests:
              - unique
              - not_null
          - name: status
            description: Note that the status can change over time

      - name: ...

  - name: ...
```

### Declaring source freshness
To configure sources to snapshot freshness information, add a `freshness` block to your source and `loaded_at_field` to your table declaration:

```yaml
version: 2

sources:
  - name: jaffle_shop
    database: raw
    freshness: # default freshness
      warn_after: {count: 12, period: hour}
      error_after: {count: 24, period: hour}
    loaded_at_field: _etl_loaded_at

    tables:
      - name: orders
        freshness: # make this a little more strict
          warn_after: {count: 6, period: hour}
          error_after: {count: 12, period: hour}

      - name: customers # this will use the freshness defined above


      - name: product_skus
        freshness: null # do not check freshness for this table
```

### Checking source freshness
To snapshot freshness information for your sources, use the `dbt source freshness` command reference docs:

```
$ dbt source freshness
```

Behind the scenes, dbt uses the freshness properties to construct a `select` query, shown below. You can find this query in the query logs.

```sql
select
  max(_etl_loaded_at) as max_loaded_at,
  convert_timezone('UTC', current_timestamp()) as snapshotted_at
from raw.jaffle_shop.orders

```

The results of this query are used to determine whether the source is fresh or not.
