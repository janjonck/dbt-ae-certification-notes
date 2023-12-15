# About the env_var function

Ref: [dbt docs](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var)

The `env_var` function can be used to incorporate Environment Variables from the system into your dbt project. This `env_var` function can be used in your `profiles.yml` file, the `dbt_project.yml` file, the `sources.yml` file, your `schema.yml` files, and in `model.sql` files. Essentially `env_var` is available anywhere dbt processes jinja code.

When used in a `profiles.yml` file (to avoid putting credentials on a server), it can be used like this:

Example `profiles.yml` file:

```yml
profile:
  target: prod
  outputs:
    prod:
      type: postgres
      host: 127.0.0.1
      # IMPORTANT: Make sure to quote the entire Jinja string here
      user: "{{ env_var('DBT_USER') }}"
      password: "{{ env_var('DBT_PASSWORD') }}"
      ....
```

If the `DBT_USER` and `DBT_PASSWORD` environment variables are present when dbt is invoked, then these variables will be pulled into the profile as expected. If any environment variables are not set, then dbt will raise a compilation error.

**!!! Be sure to quote the entire jinja string (as shown above), or else the YAML parser will be confused by the Jinja curly brackets !!!**

`env_var` accepts a second, optional argument for default value, like so:

`dbt_project.yml`:
```yml
...
models:
  jaffle_shop:
    +materialized: "{{ env_var('DBT_MATERIALIZATION', 'view') }}"
```

This can be useful to avoid compilation errors when the environment variable isn't available.

## Secrets

For certain configurations, you can use "secret" env vars. Any env var named with the prefix DBT_ENV_SECRET_ will be:

- Available for use in `profiles.yml` + `packages.yml`, via the same `env_var()` function
- Disallowed everywhere else, including `dbt_project.yml` and model SQL, to prevent accidentally writing these secret values to the data warehouse or metadata artifacts
- Scrubbed from dbt logs and replaced with `*****`, any time its value appears in those logs (even if the env var was not called directly)
- The primary use case of secret env vars is git access tokens for private packages.

Note: When dbt is loading profile credentials and package configuration, secret env vars will be replaced with the string value of the environment variable. You cannot modify secrets using Jinja filters, including type-casting filters such as `as_number` or `as_bool`, or pass them as arguments into other Jinja macros. You can only use _one secret_ per configuration:

```yml
# works
host: "{{ env_var('DBT_ENV_SECRET_HOST') }}"

# does not work
host: "www.{{ env_var('DBT_ENV_SECRET_HOST_DOMAIN') }}.com/{{ env_var('DBT_ENV_SECRET_HOST_PATH') }}"
```

