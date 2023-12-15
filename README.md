# dbt Analytics Engineer Certification notes
Consolidated notes relating to the dbt Analytics Engineering certification. This repo is structured according to the dbt certification study guide.

> - [dbt Analytics Engineer certification](https://www.getdbt.com/certifications/analytics-engineer-certification-exam)

> - [dbt certification study guide](https://www.getdbt.com/assets/uploads/dbt_certificate_study_guide.pdf)

Please make us of the [dbt docs site](https://docs.getdbt.com/docs/build/projects) which this repo is based on. This is especially relevant if you want further clarifications on anything mentioned, or if you are just curious on all what dbt has to offer. Preferably, you will have had the time to go through most of this reading material as well. The things in this repo are basically hand picked material from this site, with additional links referred to in the exam study guide.

## Checkpoint 1 - Build a foundation

- [Reading: The dbt viewpoint - how it fits in the modern analytics stack](https://docs.getdbt.com/community/resources/viewpoint)
- [dbt Project Overview](/../main/CP1-BuildAFoundation/Projects.md)
- [dbt Syntax Overview](/../main/CP1-BuildAFoundation/SyntaxOverview.md)
  - [dbt command cheat sheet](/../main/dbt_cheat_sheet.pdf)
- [dbt_project.yml file](/../main/CP1-BuildAFoundation/dbt_project.yml.md)
- [dbt Sources](/../main/CP1-BuildAFoundation/Sources.md)
  - [dbt Source Properties](/../main/CP1-BuildAFoundation/SourceProperties.md)
- [Materialization](/../main/CP1-BuildAFoundation/Materializations.md)
- [Version control - Git cheat sheet](/../main/github-git-cheat-sheet.pdf)
- [Course: dbt Fundamentals](https://courses.getdbt.com/courses/fundamentals)

## Checkpoint 2 - Modularity and Refactoring

- [Refactoring Strategies](/../main/CP2-ModularityAndRefactoring/ChooseARefactoringStrategy.md)
- [Refactoring CTEs to individual dbt models](/../main/CP2-ModularityAndRefactoring/RefactoringCTEsToIndividualModels.md)
- [Reading: How we structure our dbt projects (dbt docs)](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)
- [Reading: Your Essential dbt Project Checklist (dbt docs)](https://docs.getdbt.com/blog/essential-dbt-project-checklist#-dag-auditing)
- [Course: Refactoring SQL for Modularity](https://courses.getdbt.com/courses/refactoring-sql-for-modularity)

## Checkpoint 3 - Doing more with dbt

- [Database configs](/../main/CP3-DoingMoreWithDbt/DatabaseConfig.md)
- [Schema configs](/../main/CP3-DoingMoreWithDbt/SchemaConfig.md)
- [Target variable](/../main/CP3-DoingMoreWithDbt/TargetVariable.md)
- [Hooks](/../main/CP3-DoingMoreWithDbt/Hooks.md)
- [Exposures](/../main/CP3-DoingMoreWithDbt/Exposures.md)
- [env_var](/../main/CP3-DoingMoreWithDbt/env_var_function.md)
- [Course: Jinja, Macros, and Packages](https://courses.getdbt.com/courses/jinja-macros-packages)
- [Reading: All Jinja functions (dbt docs)](https://docs.getdbt.com/reference/dbt-jinja-functions)
  - Not mandatory to know all by heart, but some that are good to know include: `doc`, `log`, `model`, `ref`, `source`, `set`, `target`, `schema`. 

## Checkpoint 4 - Deployment and testing



