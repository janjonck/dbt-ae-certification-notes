# Schema config models

<Tabs>
<TabItem value="model" label="Model">

## Model

Specify a custom schema for a group of models in your `dbt_project.yml` file or a config block. 

For example, if you have a group of marketing-related models and you want to place them in a separate schema called `marketing`, you can configure it like this:

<File name='dbt_project.yml'>

```yml
models:
  your_project:
    marketing: #  Grouping or folder for set of models
      +schema: marketing
```
</File>

This would result in the generated relations for these models being located in the  `marketing` schema, so the full relation names would be `analytics.marketing.model_name`. 

## Seeds

Configure a custom schema in your `dbt_project.yml` file. 

For example, if you have a seed that should be placed in a separate schema called `mappings`, you can configure it like this:

<File name='dbt_project.yml'>

```yml
seeds:
  your_project:
    product_mappings:
      +schema: mappings
```

This would result in the generated relation being located in the `mappings` schema, so the full relation name would be `analytics.mappings.product_mappings`. 

## Test

Customize the schema for storing test results in your `dbt_project.yml` file. 

For example, to save test results in a specific schema, you can configure it like this:


<File name='dbt_project.yml'>

```yml
tests:
  +store_failures: true
  +schema: test_results
```

This would result in the test results being stored in the `test_results` schema.
</File>
</TabItem>
</Tabs>


## Definition
Optionally specify a custom schema for a model or seed. (To specify a schema for a snapshot, use the `target_schema` config).

When dbt creates a relation (<Term id="table" />/<Term id="view" />) in a database, it creates it as: `{{ database }}.{{ schema }}.{{ identifier }}`, e.g. `analytics.finance.payments`

The standard behavior of dbt is:
* If a custom schema is _not_ specified, the schema of the relation is the target schema (`{{ target.schema }}`).
* If a custom schema is specified, by default, the schema of the relation is `{{ target.schema }}_{{ schema }}`.


## Usage

### Models

Configure groups of models from the `dbt_project.yml` file.

<File name='dbt_project.yml'>

```yml
models:
  jaffle_shop: # the name of a project
    marketing:
      +schema: marketing
```

</File>

Configure individual models using a config block:

<File name='models/my_model.sql'>

```sql
{{ config(
    schema='marketing'
) }}
```

</File>

### Seeds
<File name='dbt_project.yml'>

```yml
seeds:
  +schema: mappings
```

</File>

### Tests

Customize the name of the schema in which tests configured to store failures will save their results:

<File name='dbt_project.yml'>

```yml
tests:
  +store_failures: true
  +schema: the_island_of_misfit_tests
```

</File>

## Warehouse specific information
* BigQuery: `dataset` and `schema` are interchangeable
