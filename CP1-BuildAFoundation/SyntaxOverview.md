# Syntax overview

For a dbt command cheat sheet please go here: [dbt command cheat sheet](/../main/dbt_cheat_sheet.pdf)

In the below text you will find explanaitons of some of these commands. All content was taken from [dbt docs (syntax)](https://docs.getdbt.com/reference/node-selection/syntax)

## Node selection

dbt's node selection syntax makes it possible to run only specific resources in a given invocation of dbt. This selection syntax is used for the following subcommands:

|    Syntax   |       Argument(s)       |
|------------:|-------------------------|
|run          | `--select`, `--exclude`, `--selector`, `--defer`                   |
|test         | `--select`, `--exclude`, `--selector`, `--defer`                   |
|seed         | `--select`, `--exclude`, `--selector`                              |
|snapshot     | `--select`, `--exclude`, `--selector`                              |
|ls (list)    | `--select`, `--exclude`, `--selector`, `--resource-type`           |
|compile      | `--select`, `--exclude`, `--selector`, `--inline`                  |
|freshness    | `--select`, `--exclude`, `--selector`                              |
|build        | `--select`, `--exclude`, `--selector`, `--resource-type`, `--defer`|
|docs generate| `--select`, `--exclude`, `--selector`                              |

# Specifying resources

## How does selection work?

1. dbt gathers all the resources that are matched by one or more of the `--select` criteria, in the order of selection methods (e.g. `tag:`), then graph operators (e.g. `+`), then finally set operators (unions, intersections, exclusions).

2. The selected resources may be models, sources, seeds, snapshots, tests. (Tests can also be selected "indirectly" via their parents; see test selection examples for details.)

3. dbt now has a list of still-selected resources of varying types. As a final step, it tosses away any resource that does not match the resource type of the current task. (Only seeds are kept for `dbt seed`, only models for `dbt run`, only tests for `dbt test`, and so on.)


# Shorthand

Select resources to build (run, test, seed, snapshot) or check freshness: `--select`, `-s`

## Examples

By default, `dbt run` will execute all of the models in the dependency graph. During development (and deployment), it is useful to specify only a subset of models to run. Use the `--select` flag with `dbt run` to select a subset of models to run. Note that the following arguments (`--select`, `--exclude`, and `--selector`) also apply to other dbt tasks, such as `test` and `build`.

### Examples of select flags

The `--select` flag accepts one or more arguments. Each argument can be one of:

1. package name
2. model name
3. fully-qualified path to a directory of models
4. selection method (`path:`, `tag:`, `config:`, `test_type:`, `test_name:`)

```yml
dbt run --select "my_dbt_project_name"   # runs all models in your project
dbt run --select "my_dbt_model"          # runs a specific model
dbt run --select "path.to.my.models"     # runs all models in a specific directory
dbt run --select "my_package.some_model" # run a specific model in a specific package
dbt run --select "tag:nightly"           # run models with the "nightly" tag
dbt run --select "path/to/models"        # run models contained in path/to/models
dbt run --select "path/to/my_model.sql"  # run a specific model by its path
```

### Examples of subsets of nodes

dbt supports a shorthand language for defining subsets of nodes. This language uses the following characters:

- plus operator (`+`)
- at operator (`@`)
- asterisk operator (`*`)
- comma operator (`,`)

```yml
# multiple arguments can be provided to --select
dbt run --select "my_first_model my_second_model"

# select my_model and all of its children
dbt run --select "my_model+"     

# select my_model, its children, and the parents of its children
dbt run --models @my_model          

# these arguments can be projects, models, directory paths, tags, or sources
dbt run --select "tag:nightly my_model finance.base.*"

# use methods and intersections for more complex selectors
dbt run --select "path:marts/finance,tag:nightly,config.materialized:table"
```

# Stateful selection

dbt operates on the assumption of stateless and idempotent operations, ensuring consistent results regardless of prior runs. While it maintains a "state" through artifacts, these are used selectively, with operations remaining stateless and idempotent. By passing file paths to the `--state` flag, dbt can identify new or modified resources, defer references to another environment, and clone nodes based on the provided manifest. This approach facilitates "slim CI," and future releases may introduce additional features leveraging the `--state` flag.

Setting State and Defer:
State and defer settings are configurable through environment variables or CLI flags:

`--state` or `DBT_STATE`: file path
`--defer` or `DBT_DEFER`: boolean
If both the flag and environment variable are specified, the flag takes precedence.

# The "result" status

Another element of job state is the `result` of a prior dbt invocation. After executing a `dbt run`, for example, dbt creates the `run_results.json` artifact which contains execution times and success / error status for dbt models.

The following dbt commands produce run_results.json artifacts whose results can be referenced in subsequent dbt invocations:

- `dbt run`
- `dbt test`
- `dbt build`
- `dbt seed`

Example:
```yml
dbt run --select "result:<status> --defer --state path/to/prod/artifacts"
```

The available options depend on the resource (node) type:

|                | model | seed | snapshot | test |
|---------------:|-------|------|----------|------|
|`result:error`  |X      |X     |X         |X     |
|`result:success`|X      |X     |X         |      |
|`result:skipped`|X      |      |X         |X     |
|`result:fail`   |       |      |          |X     |
|`result:warn`   |       |      |          |X     |
|`result:pass`   |       |      |          |X     |

# The "source_status" status

Job state includes the `source_status` from a previous dbt run. Running `dbt source freshness` generates a sources.json artifact with execution times and `max_loaded_at` dates for sources. You can find more information about `sources.json` on the 'sources' page. The commands `dbt source freshness` generate these artifacts, and you can reference their results in subsequent dbt runs by adding a selector to the command.

Example:
```yml
dbt source freshness # must be run again to compare current to previous state
dbt build --select "source_status:fresher+" --state path/to/prod/artifacts
```



