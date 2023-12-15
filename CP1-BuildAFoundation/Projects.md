# dbt Project Overview

A dbt project informs dbt about the context of your project and how to transform your data (build your data sets). By design, dbt enforces the top-level structure of a dbt project such as the `dbt_project.yml` file, the `models` directory, the `snapshots` directory, and so on. Within the directories of the top-level, you can organize your project in any way that meets the needs of your organization and data pipeline.

At a minimum, all a project needs is the `dbt_project.yml` project configuration file. dbt supports a number of different resources, so a project may also include:

| Resource  | Description  |
| :--- | :--- |
| models | Each model lives in a single file and contains logic that either transforms raw data into a dataset that is ready for analytics or, more often, is an intermediate step in such a transformation. |
| snapshots | A way to capture the state of your mutable tables so you can refer to it later. |
| seeds | CSV files with static data that you can load into your data platform with dbt. |
| data tests | SQL queries that you can write to test the models and resources in your project. |
| macros | Blocks of code that you can reuse multiple times. |
| docs | Docs for your project that you can build. |
| sources | A way to name and describe the data loaded into your warehouse by your Extract and Load tools. |
| exposures | A way to define and describe a downstream use of your project. |
| metrics | A way for you to define metrics for your project. |
| groups | Groups enable collaborative node organization in restricted collections. |
| analysis | A way to organize analytical SQL queries in your project such as the general ledger from your QuickBooks. |

## Project configuration
Every dbt project includes a project configuration file called `dbt_project.yml`. It defines the directory of the dbt project and other project configurations.

Edit `dbt_project.yml` to set up common project configurations such as:

| YAML key  | Value description  |
| :--- | :--- |
| name | Your project’s name in [snake case](https://en.wikipedia.org/wiki/Snake_case) |
| version | Version of your project |
| require-dbt-version | Restrict your project to only work with a range of dbt Core versions |
| profile | The profile dbt uses to connect to your data platform |
| model-paths | Directories to where your model and source files live  |
| seed-paths | Directories to where your seed files live |
| test-paths | Directories to where your test files live |
| analysis-paths | Directories to where your analyses live |
| macro-paths | Directories to where your macros live |
| snapshot-paths | Directories to where your snapshots live |
| docs-paths | Directories to where your docs blocks live |
| vars | Project variables you want to use for data compilation |



## Project subdirectories

You can use the Project subdirectory option in dbt Cloud to specify a subdirectory in your git repository that dbt should use as the root directory for your project. This is helpful when you have multiple dbt projects in one repository or when you want to organize your dbt project files into subdirectories for easier management.

To use the Project subdirectory option in dbt Cloud, follow these steps:

1. Click on the cog icon on the upper right side of the page and click on **Account Settings**.

2. Under **Projects**, select the project you want to configure as a project subdirectory.

3. Select **Edit** on the lower right-hand corner of the page.

4. In the **Project subdirectory** field, add the name of the subdirectory. For example, if your dbt project files are located in a subdirectory called `<repository>/finance`, you would enter `finance` as the subdirectory. 

    * You can also reference nested subdirectories. For example, if your dbt project files are located in `<repository>/teams/finance`, you would enter `teams/finance` as the subdirectory. **Note**: You do not need a leading or trailing `/` in the Project subdirectory field. 

5. Click **Save** when you've finished.

After configuring the Project subdirectory option, dbt Cloud will use it as the root directory for your dbt project. This means that dbt commands, such as `dbt run` or `dbt test`, will operate on files within the specified subdirectory. If there is no `dbt_project.yml` file in the Project subdirectory, you will be prompted to initialize the dbt project.
