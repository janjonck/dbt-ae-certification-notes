## Overview

Ref: [dbt docs](https://docs.getdbt.com/docs/build/data-tests)

Data tests are assertions you make about your models and other resources in your dbt project (e.g. sources, seeds and snapshots). When you run `dbt test`, dbt will tell you if each test in your project passes or fails.

You can use data tests to improve the integrity of the SQL in each model by making assertions about the results generated. Out of the box, you can test whether a specified column in a model only contains non-null values, unique values, or values that have a corresponding value in another model (for example, a `customer_id` for an `order` corresponds to an `id` in the `customers` model), and values from a specified list.

Data tests return a set of failing records. Generic data tests (f.k.a. schema tests) are defined using `test` blocks.

Like almost everything in dbt, data tests are SQL queries. In particular, they are `select` statements that seek to grab "failing" records, ones that disprove your assertion. If you assert that a column is unique in a model, the test query selects for duplicates; if you assert that a column is never null, the test seeks after nulls. If the data test returns zero failing rows, it passes, and your assertion has been validated.

There are two ways of defining data tests in dbt:
* A **singular** data test is testing in its simplest form: If you can write a SQL query that returns failing rows, you can save that query in a `.sql` file within your test directory. It's now a data test, and it will be executed by the `dbt test` command.
* A **generic** data test is a parameterized query that accepts arguments. The test query is defined in a special `test` block (like a macro(jinja-macros)). Once defined, you can reference the generic test by name throughout your `.yml` files—define it on models, columns, sources, snapshots, and seeds. dbt ships with four generic data tests built in, and we think you should use them!

## Singular data tests

The simplest way to define a data test is by writing the exact SQL that will return failing records. We call these "singular" data tests, because they're one-off assertions usable for a single purpose.

These tests are defined in `.sql` files, typically in your `tests` directory (as defined by your `test-paths` config). You can use Jinja (including `ref` and `source`) in the test definition, just like you can when creating models. Each `.sql` file contains one `select` statement, and it defines one data test:

```sql
-- Refunds have a negative amount, so the total amount should always be >= 0.
-- Therefore return records where this isn't true to make the test fail
select
    order_id,
    sum(amount) as total_amount
from {{ ref('fct_payments' )}}
group by 1
having not(total_amount >= 0)
```

## Generic data tests
Certain data tests are generic: they can be reused over and over again. A generic data test is defined in a `test` block, which contains a parametrized query and accepts arguments. It might look like:

```sql
{% test not_null(model, column_name) %}

    select *
    from {{ model }}
    where {{ column_name }} is null

{% endtest %}
```

You'll notice that there are two arguments, `model` and `column_name`, which are then templated into the query. This is what makes the test "generic": it can be defined on as many columns as you like, across as many models as you like, and dbt will pass the values of `model` and `column_name` accordingly. Once that generic test has been defined, it can be added as a _property_ on any existing model (or source, seed, or snapshot). These properties are added in  `.yml` files in the same directory as your resource.

Out of the box, dbt ships with four generic data tests already defined: `unique`, `not_null`, `accepted_values` and `relationships`. Here's a full example using those tests on an `orders` model:

```yml
version: 2

models:
  - name: orders
    columns:
      - name: order_id
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['placed', 'shipped', 'completed', 'returned']
      - name: customer_id
        tests:
          - relationships:
              to: ref('customers')
              field: id
```

In plain English, these data tests translate to:
* `unique`: the `order_id` column in the `orders` model should be unique
* `not_null`: the `order_id` column in the `orders` model should not contain null values
* `accepted_values`: the `status` column in the `orders` should be  one of `'placed'`, `'shipped'`, `'completed'`, or  `'returned'`
* `relationships`: each `customer_id` in the `orders` model exists as an `id` in the `customers` table (also known as referential integrity)
