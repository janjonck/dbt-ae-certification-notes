**Analytics Pull Request Review Checklist:**

✅ **Represents one logical unit of work:**
   - Avoid combining unrelated changes to enhance review clarity.
   - Break up extensive changes into modular components.

✅ **Description provides context:**
   - Submitter writes a detailed rationale for the change.
   - Set the context for reviewers before diving into code.

✅ **Continuous integration tests have completed successfully:**
   - CI environment tests the full project for errors.
   - Ensure a green check indicates successful testing.

✅ **Code adheres to the project’s style guide:**
   - Maintain a consistent coding style within the team.
   - Suggest updates if the code deviates from the style guide.

✅ **Every new model has a test:**
   - Confirm that each new model in the PR has a corresponding test.
   - Test for uniqueness and ensure the unique key is not null.

✅ **Documentation is added:**
   - Add documentation for complex changes.
   - Explain data intricacies while the transformation logic is fresh.

✅ **Code is DRY (Don't Repeat Yourself):**
   - Review for code repetition and duplication.
   - Consider refactoring for long-term project health.

✅ **Data returns expected results:**
   - Query models in the CI environment to familiarize with the work.
   - Check for efficiency, correct joins, and altered data grain.

✅ **Models are materialized appropriately:**
   - Materialize models as tables if intended for BI layer querying.
   - Consider warehouse-specific requirements (e.g., Redshift sort and dist keys).

✅ **Naming is clear and consistent:**
   - Ensure model names represent the correct grain.
   - Follow project conventions for model prefixes.
   - Review newly created field names for clarity.

⚡️ **Tip: Create a pull request template in your project:**
   - Use a standardized template for all analytics code PRs.
   - Include reminders for each checklist item to improve PR quality.
