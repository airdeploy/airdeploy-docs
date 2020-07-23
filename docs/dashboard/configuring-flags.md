---
id: configuring-flags
title: Configuring Flags
sidebar_label: Configuring Flags
---

Each flag has a detailed configuration page where you can fine tune details about how a flag behaves. Although flags can be simple on/off toggles, they can also have more complex targeting configurations (see [Targeting](../concepts/targeting.md)).

The flag configuration page visualizes the flow of traffic through the flag switch point. This diagram is interactive, and serves as an editor. Changes are not propagated until changes are saved, so the editor allows you to "draft" changes.

> Only Editors, Admins, and Owners can modify flag configurations. Viewers have read-only access to flags.

## Subpopulations

By default, all entity traffic is directed straight to the default variation. However, by creating subpopulations, you can divert traffic into the flag.

For example, when launching a new feature to 10% of all Users, a subpopulation is created for Users. By default, the size of that population is 0. You can control the percentage of entities (or the size of the subpopulation) by adjusting the slider (or text input).

![Subpopulation Sampling](assets/adjust-es.png)

This controls the size of the population being diverted through the flag.

### Adding a Subpopulation

Although there is a default subpopulation on every flag, additional subpopulations can be added to the flag, diverting more traffic from the default path.

- Click "Add a Subpopulation" underneath the existing subpopulation(s)
- A new subpopulation node will appear

### Editing a Subpopulation

Subpopulations are defined by not only an entity type (such as all Users), but a set of filter criteria. You can add filters to further narrow down the subpopulation size.

- Click on a subpopulation node
- A panel will open on the right with details of the subpopulation
- Make changes in the detail panel

![Subpopulation Editing](assets/create-subset.png)

### Deleting a Subpopulation

- Click on the three dots on the subpopulation
- Click delete in the popup
- The subpopulation will be removed

## Variations

Flags function as switch points to route traffic between variations. By default, traffic flows directly into the default variation (which represents the "off" state of a flag).

Traffic routed through a flag is directed towards flag variations. When first created, flags contain only one flag variation that represents an "on" state.

### Adding a Variation

However, variations can be edited. For example, if your feature has more than one variant, additional variations can be created.

- Click "Add a Variation" at the bottom of the variations list

### Rebalancing Variations

Traffic that reaches the flag is split between the variations. By default, the traffic is split evenly between the variations. This means that there is an equal probability that an entity will be assigned to each entity.

However, you can rebalance the percentages as long as they total to 100%.

![Rebalancing](assets/balance-v.png)

- Click the variation probability text input you wish to rebalance
- Type the new value
- Adjust the other variation probability text inputs to achieve 100% allocation

### Deleting a Variation

- Click on the three dots on the variation
- Click delete in the popup
- The variation will be removed

> Note: If you still have more than one variation, you may need to [rebalance your variations](#rebalancing-variations).

## Whitelist

Whitelisting is a way to short-circuit the flag's randomization logic. Instead, you can assign a specific entity to a specific variation. Once assigned, the entity will always receive that specific variation.

A badge on each variation indicates how many entities have been whitelisted to that variation.

![Whitelist](assets/wl-entities.png)

### Whitelisting an Entity

- Click the variation you wish to add the entity to
- A side panel appears showing variation details
- In the Whitelisted Entities section, you can:
  - Search for an entity by name or ID
  - Narrow the search by type
- Choose an available entity from the resulting dropdown
- The entity will then be added to the whitelist

![Whitelist an Entity](assets/wl-entity.png)

### Removing a Whitelisted Entity

- Click the feature variation you wish to remove the entity from
- A side panel appears showing variation details
- Click the “X” next to the entity
