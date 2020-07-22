---
id: groups
title: Groups
sidebar_label: Groups
---

Entities can also be part of groups, which is useful for inheriting flag states. When groups receive a particular feature, all of it's children entities inherit the same feature permissions as well. For example, if your customers are grouped into companies, you could target some features to companies and all the employee would inherit the company's feature set.

Group entities are also entities, and have the same structure as individual entities. They are simply nested within an entity under the group property (to associate an entity to a group).

Groups can be targeted the same way any other entities are targeted: randomly sampled, whitelisted, blacklisted, etc.

## Creating Groups

Group relationships are created by nesting the entity representing the group within the entity.

In the example below, the User entity is associated with a group (a Club entity):

```json
{
  "type": "User",
  "id": "1234",
  "displayName": "ironman@stark.com",
  "attributes": {
    "tShirtSize": "M",
    "dateCreated": "2018-02-18",
    "timeConverted": "2018-02-20T21:54:00.630815+00:00",
    "ownsProperty": true,
    "age": 39
  },
  "group": {
    "type": "Club",
    "id": "5678",
    "displayName": "Avengers Club",
    "attributes": {
      "founded": "2016-01-01",
      "active": true
    }
  }
}
```

As long as the group is nested, the entity can inherit their flag assignment from their parent.

## Precedence

If an individual entity is prescribed any particular flag variation, or is targeted in any way, it takes precedence over the group's assignment. The group assignment always yields to the individual's assignment, if one is specified.

For example, if an entire company is sampled to receive a feature, but a particular user that belongs to that company is explicitly blacklisted from receiving that feature, that blacklist will take precedence over the group's inclusion for that user.
