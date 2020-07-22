---
id: entities
title: Entities
sidebar_label: Entities
---

- See [What is an entity?](work.md#what-is-an-entity) for a definition and how they work.
- See [Environments](environments.md) to understand how entities are different in each environment.

## Adding Entities

Alvin to add

## Uploading/Syncing Entities

Alvin to add

## Adding Custom Entity Fields

Alvin to add

## Inspecting Entities

`Main Navigation, Project Menu > Choose Entities`

![](assets/inspecting-ent.png)

By default, where entities are detected, all entities are listed by Type, ID, Name, when they were last served a Feature Variation, and if they are a whitelisted entity.

The left navigation panel has a few capabilities:

- **Find Entities** — a way to search for a specific entity in the list. You may search on any of the columns listed.
- **Only Show in Groups** — a way to cluster entities by their common attributes
- **Type Filters** — These are the types Airdeploy detected in your Entity list. There is a filter for each type. Clicking it restricts the list to that type.

The main view of the Entities screen is the list of entities (filtered or not) and an Entity detail. Clicking on any Entity will load its detail.

![](assets/entity.png)

There are a few constant sections of the Entity detail and the rest is custom. You may add as many custom fields to
your entities to suit your needs (see [Adding Custom Entity Fields](#adding-custom-entity-fields)).

## Entity Summary

This includes the basic Entity information. Name, Type, ID, Last seen, and when the Entity was created.

## Entity Relationships

Displays the parent entity (if exists), and lists the children (if exists). This area is expandable to show the entity details.

## Additional Attributes

These are custom fields and their values (see [Adding Custom Entity Fields](#adding-custom-entity-fields)).

## Whitelisted Flags

This lists the Flags and Feature variations where this Entity is whitelisted.

You may add this Entity to other Flags in the Project.

- Locate the Flag in the list, or filter the flag list through search
- Enable the switch
- Choose any Feature variation. By default, the “Off” Feature variation is selected.
