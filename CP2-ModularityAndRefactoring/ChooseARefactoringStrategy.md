# Choosing a refactoring strategy

There are two ways you can choose to refactor: in-place or alongside.

Note: before reading this, you should know that dbt generally recommend the alongside refactoring method.

This info is from [dbt docs refactoring legacy sql guide](https://docs.getdbt.com/guides/refactoring-legacy-sql?step=4) - here you will also find a 03:20 minute video from the 'Refactoring legacy SQL to dbt' guide.

## In-place refactoring

Means that you will work directly on the SQL script that you ported over in the first step.

You'll move it into a `/marts` subfolder within your project's `/models` folder and go to town.

### Pros:

- You won't have any old models to delete once refactoring is done.

### Cons:

- More pressure to get it right the first time, especially if you've referenced this model from any BI tool or downstream process.
- Harder to audit, since you've overwritten your audit comparison model.
- Requires navigating through Git commits to see what code you've changed throughout.

## Alongside refactoring

Means that you will copy your model to a `/marts` folder, and work on changes on that copy.

### Pros:

- Less impact on end users - anything that is referencing the model you're refactoring can keep that reference until you can safely deprecate that model.
- Less pressure to get it right the first time, meaning you can push/merge smaller PRs. This is better for you and your reviewers.
- You can audit easier by running the old and new models in your dev branch and comparing the results. This ensures the datasets you're comparing have the same or very close to the same records.
- You can look at old code more easily, as it has not been changed.
- You can decide when the old model is ready to be deprecated.

### Cons:

- You'll have the old file(s) in your project until you can deprecate them - running side-by-side like this can feel duplicative, and may be a headache to manage if you're migrating a number of queries in bulk.

We generally recommend the alongside approach.
