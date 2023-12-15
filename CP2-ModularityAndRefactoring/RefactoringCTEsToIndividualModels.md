# Implement CTE Groupings

This info is from [dbt docs - Refactoring legacy SQL to dbt - Guide](https://docs.getdbt.com/guides/refactoring-legacy-sql?step=5)
Here you will also find a 10:47 minute on 'CTE Grouping and cosmetic cleanups' from the guide.

If your interested in dbt's own style guide for all supported languages, click [here](https://docs.getdbt.com/best-practices/how-we-style/0-how-we-style-our-dbt-projects#why-does-style-matter).

## Converting legacy SQL in dbt

Once you choose your refactoring strategy, you'll want to do some cosmetic cleanups according to your data modeling best practices and start moving code into CTE groupings. This will give you a head start on porting SQL snippets from CTEs into modular dbt data models. This can be done in the form of staging or intermediate models, `models/staging` or `models/intermediate`. This way, a CTE from an old setup/project might become its own staging or intermediate model in dbt.

## What's a CTE?

CTE stands for “Common Table Expression”, which is a temporary result set available for use until the end of SQL script execution. Using the `with` keyword at the top of a query allows us to use CTEs in our code.

Inside of the model we're refactoring, we’re going to use a 4-part layout:

1. 'Import' CTEs
2. 'Logical' CTEs
3. A 'Final' CTE
4. A simple SELECT statement

In practice this looks like:
```sql
with

import_orders as (

    -- query only non-test orders -> this CTE will become a staging model
    select * from {{ source('jaffle_shop', 'orders') }}
    where amount > 0
),

import_customers as (
    -- This will also become a staging model
    select * from {{ source('jaffle_shop', 'customers') }}
),

logical_cte_1 as (

    -- perform some math on import_orders
    -- potential intermediate model

),

logical_cte_2 as (

    -- perform some math on import_customers
    -- potential intermediate model
),

final_cte as (

    -- join together logical_cte_1 and logical_cte_2
)

select * from final_cte
```

### Import CTEs

In the example above, we have two "import CTEs" where we are selecting directly from the source with minor or no transformations. These are good examples of CTEs we would most likely want to turn into `staging` models. We want to look at the transformations that can occur within each of these sources without needing to be joined to each other, and then we want to make components out of those so they can be our building blocks for further development.

### Logical CTEs

Logical CTEs contain unique transformations used to generate the final product, and we want to separate these into logical blocks. To identify our logical CTEs, we will follow subqueries in order.
If a subquery has nested subqueries, we will want to continue moving down until we get to the first layer, then pull out the subqueries in order as CTEs, making our way back to the final select statement.
Name these CTEs as the alias that the subquery was given - you can rename it later, but for now it is best to make as few changes as possible.
If the script is particularly complicated, it's worth it to go through once you're finished pulling out subqueries and follow the CTEs to make sure they happen in an order that makes sense for the end result.
When a model becomes complex or can be divided out into reusable components you may consider an intermediate model. Intermediate models are optional and are not always needed, but do help when you have large data flows coming together.

### Final CTE

The previous process usually results in a select statement that is left over at the end - this select statement can be moved into its own CTE called the final CTE, or can be named something that is inherent for others to understand. This CTE determines the final product of the model.

### Simple select statement

After you have moved everything into CTEs, you'll want to write a `select * from final` (or something similar, depending on your final CTE name) at the end of the model.

This allows anyone after us to easily step through the CTEs when troubleshooting, rather than having to untangle nested queries.
