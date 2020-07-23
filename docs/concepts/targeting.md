---
id: targeting
title: Targeting Methods
sidebar_label: Targeting Methods
---

There are several ways to target entities in Airdeploy. By mixing and matching them, they can express many intended ways of routing traffic through a flag.

## Subpopulation Sampling

This method randomly selects a percentage of a subpopulation (a subset of the traffic going through a flag). A percentage of the entities that meet certain criteria will be receive the feature behind the flag (be assigned one of the active variations).

In this example, the entire universe of users is broken into several subsets (aka subpopulations).

![Subpopulation Sampling](assets/entity-subset-work.png)

Every subpopulation has a sampling percentage, which informs the flag to allow through a random sample of entity traffic in that subpopulation.

A subpopulation could logically represent more complex situations. A few examples:

- 30% of all users in Luxembourg
- 10% of all users under the age of 25 AND do not own cars
- 100% of all companies with < 50 employees

### Constructing a Subpopulation

A subpopulation is first defined by a set of filters, which determines which entities are to included in the subpopulation.

For example, a subpopulation could be filtered to only users where "operating system is not MacOS". In this example, it may be that the original pool of user entities was 3,000. With the filter applied, it may be reduced to 1,200. Only these entities will be sampled.

In each subpopulation, multiple filters can be chained together - aka connected with "AND" logical operations. For example, from the previous example of a user entity subset where "operating system is not MacOS", you could add another filter where "state is Iowa". The resulting subpopulation is limited to only users who meet both criteria - and therefore only those users would be sampled according to the sampling percentage.

### Combining Subpopulations

In the examples above, a single subpopulation was defined, diverting only entities that meet that subpopulation's criteria.

However, multiple subpopulations can be defined. In these cases, if an entity meets **any** of the subpopulations criteria, it will be considered for sampling.

Order matters here - the first subpopulation that the entity matches (according to the order defined in the dashboard) is the subpopulation that the entity is considered to be a part of.

> In logic speak, subpopulation filters are `AND`ed together. To qualify as a member of a subpopulation, an entity must meet all the filter criterion.
> Multiple subpopulations are `OR`ed together. That is, an entity only needs to be in one of the sample subpopulations to receive the feature behind the flag.

> In logic terminology, this is known as **disjunctive normal form**. It turns out that all logical formulas can be converted into an equivalent disjunctive normal form. This is handy for constructing complex audiences using Subpopulations.

## Whitelisting

Typically, entities flow through subpopulation filters through the flag variations. They are assigned to variations randomly based on the percentages assigned (either evenly split by default or by your custom percentages). This is useful in most cases. However, there may be certain entities you wish to assign to a specific variation.

![wWhitelisting](assets/wl-bl.jpeg)

Whitelisting is a way to ensure that a particular entity (or group of entities) receives a particular variation of the flag.

For example, the developer of a feature may want to ensure, with certainty, that they always receive the feature under development. In that case, they would whitelist themselves to the `on` variation of the flag.

You can assign any entity to a specific variation and they will see that variation (as long as the flag is on). You can also assign any entity directly to the default variation. In that case, they will never see any variation regardless of the flag being on or off.

## Order of Precedence

The combined order of precedence for determining variations is:

1. Kill Switch: Default variation if the flag is "killed" (kill switch engaged)
2. Individual Whitelist: The specified, whitelisted variation assigned to an individual entity
3. Group Whitelist: The specified, whitelisted variation assigned to the entity's parent group
4. Individual Subpopulation: One of the flag's variations is randomly assigned based on the split provided.
5. Group Subpopulation: Variation assigned to the entity's parent group through randomization
6. Default Variation: Off by default
