# Adding exposures to your DAG/dbt project

Ref: [dbt docs](https://docs.getdbt.com/docs/build/exposures)

Exposures make it possible to define and describe a downstream use of your dbt project, such as in a dashboard, application, or data science pipeline. By defining exposures, you can then:

- run, test, and list resources that feed into your exposure
- populate a dedicated page in the auto-generated documentation site with context relevant to data consumers

## Declaring an exposure

Exposures are defined in `.yml` files nested under an `exposures:` key.

Example `models/<filename>.yml`:

```yml
version: 2

exposures:

  - name: weekly_jaffle_metrics
    label: Jaffles by the Week
    type: dashboard
    maturity: high
    url: https://bi.tool/dashboards/1
    description: >
      Did someone say "exponential growth"?

    depends_on:
      - ref('fct_orders')
      - ref('dim_customers')
      - source('gsheets', 'goals')
      - metric('count_orders')

    owner:
      name: Callum McData
      email: data@jaffleshop.com
```

## Available properties

_Required:_

- **name**: a unique exposure name written in [snake case](https://en.wikipedia.org/wiki/Snake_case)
- **type**: one of `dashboard`, `notebook`, `analysis`, `ml`, `application` (used to organize in docs site)
- **owner**: `name` or `email` required; additional properties allowed

_Expected:_

- **depends_on**: list of refable nodes, including `ref`, `source`, and `metric` (While possible, it is highly unlikely you will ever need an `exposure` to depend on a `source` directly)

_Optional:_

- **label**: may contain spaces, capital letters, or special characters.
- **url**: enables the link to View this exposure in the upper right corner of the generated documentation site
- **maturity**: one of `high`, `medium`, `low`

_General properties (optional)_

- **description**
- **tags**
- **meta**

## Referencing exposures

Once an exposure is defined, you can run commands that reference it:

```yml
dbt run -s +exposure:weekly_jaffle_report
dbt test -s +exposure:weekly_jaffle_report
```

When we generate our documentation site, you'll see the exposure appear as orange nodes in your DAG.
