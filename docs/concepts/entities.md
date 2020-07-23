---
id: entities
title: Entities
sidebar_label: Entities
---

An **entity** is an object that flows through a flag switch point and are targeted by feature flags. They are represented by nouns that represent objects that should be allocated between feature variations.The most common entity is a **user**, which is a built-in type of entity in Airdeploy, but other things can be entities as well. Here are a few examples:

- Users (default)
- Addresses
- Organizations
- Locations
- Departments
- Product Items

Entities are the primary input by which you allow Airdeploy to return an answer of what feature variation should be displayed or executed.

Entities are minimally comprised of:

- **Identifier** (required) - A unique identifier for the user (i.e. user ID, email, username)
- **Type** (optional) - The category of the entity. If excluded, defaults to built-in type of **User**
- **Name** (optional) - A human-readable identifier for the entity

## Users

Users are a built-in entity type in Airdeploy. By default, entities will default to type `User` if no value is provided.

## Attributes

Entities are also allowed a user-defined **attributes** map. Attributes are used to filter and target specific entities. Attribute values can be any of the following types:

| Type            | Description                       |
| --------------- | --------------------------------- |
| String          | Any string value                  |
| Boolean         | A boolean (true / false) value    |
| Number          | An integer or float               |
| Date / Datetime | An ISO 8601-formatted date string |

**An example entity with some attributes:**

```json
{
  "type": "User",
  "id": "639482",
  "displayName": "Danaerys Targaeryen",
  "attributes": {
    "email": "dany@ironthronehr.com",
    "first_name": "Danaerys",
    "last_name": "Targaeryen",
    "date_of_birth": "1988-09-09",
    "age": 29,
    "is_married": false,
    "ip_address": "191.191.191.191",
    "city": "San Francisco",
    "state": "CA",
    "country": "USA",
    "date_created": "2017-12-10T08:12:44.987631+00:00",
    "favorite_drink": "coffee",
    "favorite_book": "The Hitchhiker's Guide to the Galaxy"
  }
}
```

## Groups / Relationships

Entities can be segmented into groups. Groups allow modeling of parent-child relationships between entities. Enabling a feature for a group, for example, will enable the feature for all it's children.

## Entity Model Reference

In most languages, entities are represented as a map, dictionary, struct, or comparable data structure. Each language's documentation specifies more clearly how to represent entities, but they share these common components:

| Field       | Input Type          | Description                                                                                                                                                        |
| ----------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type        | String              | **Optional** (defaults to "User") Type of this Entity. Example: 'User', 'Company', 'Group', 'Page, 'Product Listing'                                               |
| id          | String              | **Required**. A unique identifier. We recommend using your internal identifier (such as object/document ID).                                                       |
| displayName | String              | **Required**. A human-readable display name. This can be a username, a full name, email, etc.                                                                      |
| attributes  | Map / Dictionary    | **Optional**. Key-value pairs that contains attributes that can be used in Airship to target specific users or objects. Read about Entities & Users to learn more. |
| group       | Object / Dictionary | **Optional**. Associate an entity with a group by nesting the entity that represents the parent entity.                                                            |
