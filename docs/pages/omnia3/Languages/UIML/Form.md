---
title: OMNIA 3
keywords: omnia3
summary: "OMNIA 3 UIMLForm"
sidebar: omnia3_sidebar
permalink: omnia3_languages_UIMLForm.html
folder: omnia3
---

# Form
Form to manage a given Entity Definition.
## Properties

| Name | Type | Aggregation Kind | Multiplicity | Description |
| --------- | --------- | --------- | --------- | --------- |
| Name | Text | None | 1..1 | The name of the entity (unique identifier). |
| Description | Text | None | 0..1 | The textual explanation of the entities’ purpose. |
| Label | Text | None | 1..1 | Label to display in the application. |
| HelpText | Text | None | 0..1 | Text/annotation to help the user. |
| Type | ElementType | None | 1..1 |  |
| Attributes | ElementAttribute | Composite | 0..* |  |
| Behaviours | ElementBehaviour | Composite | 0..* |  |
| Elements | InnerElement | Composite | 0..* |  |
| Entity | Text | None | 1..1 | Entity to manage. |
| DataSource | Text | None | 0..1 | The Data Source in which the entities are computed and/or persisted |

## Operations

| Name | Type | Description |
| --------- | --------- | --------- |
| Defaults | Initialize |  |

