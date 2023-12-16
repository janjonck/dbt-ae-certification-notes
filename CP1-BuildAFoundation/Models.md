## Overview
dbt Core and Cloud are composed of different moving parts working harmoniously. All of them are important to what dbt does — transforming data—the 'T' in ELT. When you execute `dbt run`, you are running a model that will transform your data without that data ever leaving your warehouse.

Models are where your developers spend most of their time within a dbt environment. Models are primarily written as a `select` statement and saved as a `.sql` file. While the definition is straightforward, the complexity of the execution will vary from environment to environment. Models will be written and rewritten as needs evolve and your organization finds new ways to maximize efficiency.

SQL is the language most dbt users will utilize, but it is not the only one for building models. Starting in version 1.3, dbt Core and dbt Cloud support Python models. Python models are useful for training or deploying data science models, complex transformations, or where a specific Python package meets a need — such as using the `dateutil` library to parse dates.

## Models and modern workflows
The top level of a dbt workflow is the project. A project is a directory of a `.yml` file (the project configuration) and either `.sql` or `.py` files (the models). The project file tells dbt the project context, and the models let dbt know how to build a specific data set.
