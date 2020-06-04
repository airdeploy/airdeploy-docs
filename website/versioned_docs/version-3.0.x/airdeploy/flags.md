---
id: version-3.0.x-flags
title: Flags
sidebar_label: Flags
original_id: flags
---

## Flag Control

Just like in the __Managing Flags__, you can [Enable and Disable a Flag](projects.md#enable-and-disable-a-flag) to control whether entities receive their assigned variation or are all directed to Off. However, you must save and publish your choice for it to take effect.

- Locate the toggle in the Flag Control panel.
- Toggle the on or off.
- Click “Save & Publish” for the flag to be enabled or disabled.

> Note: In order for any changes to the flag (including modifying entities and variations) to take effect, you must click “Save & Publish” in Flag Control.

## Entities & Subsets

All flags need an Entity Source in order to function. By default, Airdeploy creates a User type Entity Source if it detects user entities in your list.
 
### Adjusting Entity Source strength

![](assets/adjust-es.png)


By default, all Entity Sources are at 100%. This means that all the entities of that type will be assigned to one of the variations. You can control the percentage of all your entities that will be assigned by using the slider (or text input) on the Entity Source.

The source strength only controls the volume of entities passing through your flag. For more specific control, see [Creating a subset](#creating-a-subset) and Whitelisting & Blacklisting.

### Creating a subset

![](assets/create-subset.png)

You can hone in on certain properties of your entity type by creating a subset through filtering. Each filter consists of an attribute (populated by your entity list), and operator, and a target value (populated based on your entity list or the type of the attribute).

You can add as many filters as you need to create the specific subset you need.


- Hover over the Entity Source. Attribute, operator, and value dropdown will appear.
- Select the options you want for each dropdown.
- Add additional filters by making selections on the new row of attribute, operator, and value dropdowns.

### Removing a subset

![](assets/remove-ss.png)

- Hover over the filter you wish to remove
- Click the “X”
- The filter is removed.

## Variations

With a default flag, you can control whether entities flow through your feature or not. This binary action allows you to effectively turn features on or off from a software perspective.

The power of Airdeploy is magnified when you leverage feature variations. See 
[How do flags and variations work](work.md#how-do-flags-and-variations-work)? for more explanation on the function.

### Renaming a variation

- Click the variation. A side panel appears.
- In the side panel, make changes to the name, description, or [whitelisted entities](#whitelisting-an-entity).
> Note: Changing the name will change the code-reference name. __This will require a modification to the Airdeploy code snippet in your code and a redeployment__.

### Adding a variation

- Click “Add a feature variation” at the bottom of the variations panel. A side panel appears.
- In the side panel, make changes to the name, description, or whitelisted entities.
> Note: Adding a variation will change the code-reference name. __This will require a modification to the Airdeploy code snippet in your code and a redeployment__.

### Balancing variations

![](assets/balance-v.png)


When you have two or more variations, Airdeploy automatically balances their probability evenly. This means that there is an equal probability that the combined Entity Sources will be served one variation over the other.

You may rebalance the percentages to suit your situation as long as they total 100%. Realtime feedback will let you know if you are over-, under-, or exactly allocated.


- Click the variation probability text input you wish to rebalance.
- Type the new value. As you type you will see the realtime indicator update.
- Adjust the other variation probability text inputs to achieve 100%.

### Removing a variation

![](assets/remove-v.png)

- Click on three dots. 
- Click the “Delete” option in the popup that appears.
- The variation will be removed.
> Note: If you still have more than one variation, you may need to [rebalance your variations](#balancing-variations).
  
> Note: Removing a variation will change the code-reference name. __This will require a modification to the Airdeploy code snippet in your code and a redeployment__.


## Whitelisting Entities

When Entity subsets are not specific enough for your purposes, you may want to assign specific entities to specific feature variations. When you assign an entity to a variation, they will always receive that variation. When you assign a specific entity to the “Off” flag variation it will ensure they never see any of the feature variations.

A badge displays the number of entities whitelisted for that variation.


![](assets/wl-entities.png)


### Whitelisting an entity

![](assets/wl-entity.png)

- Click the feature variation you wish to add the entity to (this include the “Off” flag variation). A side panel appears. 
- In the Whitelisted Entities section, you can:
    - Search for an entity by name or ID
    - Narrow the search by type
- Choose an available entity from the resulting dropdown
- The entity is added to the Whitelist.

### Removing a Whitelisted entity

- Click the feature variation you wish to remove the entity from.A side panel appears.
- Hover over the entity you wish to remove. 
- Click the “X” that appears.

## Preview

As you configure your flag, you may want to see how the entities be assigned. This panel will update in realtime as you modify entities, subsets, feature variations, and whitelisting.

![Flag Preview Table](assets/t-a.png)


### Assignment Relationship Chart

Alvin to review -> do we still have it? Looks like no

The left panel displays an interactive chart showing where entities will be assigned  when the flag is active.

The left column displays the number of entities resulting from any Entity Source strength or Subset filters you configured (Entity Sources are identified by letter badge).

The right column displays the Feature variations and the count of entities being assigned to that variation.

Clicking on any Entity source in the left column will highlight the Feature variations those entities will be assigned. The numbers on each Feature variation in the right column will accurately represent the percentage probability assigned to each Feature variation.

Clicking on any Feature variation in the right column will highlight the Entity Sources feeding that Feature variation. The numbers on each Entity source will accurately represent the percentage probability assigned to that Feature variation.

### Assignment Detail Table

The right panel displays a dynamic table showing the specific entities and the Feature variations they are assigned. Regardless of which element is clicked in the assignment relationship chart, the table will update automatically and show the complete list of entities.

This table is sortable on Feature variation, Entity Type, Entity ID, and Entity Name. You can also find an entity by searching. Entities that are waitlisted are indicated by a white listing icon in the ID column.

## Flag History

Flag history is a way of seeing all of the “Save & Publish” sessions committed on a flag. It is organized by date and time stamp as well as the team member associated with the session.

Each published session is detailed by what was added, removed, or changed. 

### Commenting on a published session

- Hover over the Date and Time stamp. A comment button will appear.
- Click on the “Add Comment” button. A side panel will appear.
- Add your comment.
- Click “Save Comment”
> Note: Comments are not threaded.

## Flag Experiments

While the Traffic Assessment Preview allows you to anticipate which users will be assigned to what flag variation, a Flag Experiment allows you to test variations against one or more metrics.

### Experiment Metrics

Experiments are run using one or more metrics. Metrics are even hooks you have access through from your Analytics Source. In AirDepoly, Metrics have one of three types:

- Percent (%)
- Count (#)
- Sum (+)
![](assets/exp-metric.png)


To view the results for a given Metric, select it from this menu. You may always add another metric, but data is collected on the date you add it to the Experiment.

For more about how Metrics relate to Projects, see [Metrics](projects.md#metrics).

### Session Collection

Metric data is collected the moment you add a metric to the Experiment. Data is continually collected in a rolling 90-day period or until you delete the Metric. You can limit the experiment analysis to a date range anywhere within that 90-day period.

![](assets/session-c.png)


### Feature Variation Analysis

For each Feature Variation in your flag, there is a brief analysis using the samples in the Session Collection date range:

- Win Probability—For the selected metric, the probability this variation wins over the other variations
- Statistical Highlights—Typical dimensions of Mean, Percentile Range, and Sample Size.
- Distribution Chart—A visual overlay of the Feature Variation’s samples.


![](assets/f-a.png)


Clicking any Feature Variation summary will highlight its data in the chart.

![](assets/f-s.png)


### Sample Detail Panel

Every entity that passed through the flag and was assigned a Feature Variation is displayed in a Sample Detail Panel. You can see the entity’s value in the experiment.

![](assets/s-d.png)


### Creating an Experiment

- Navigate to a Flag
- Choose “Experiments” from the left navigation.
- Add one or more metrics (see [Creating a Metric on a Flag Experiment](projects.md#creating-a-metric-on-a-flag-experiment)).

### Deleting an Experiment

- Remove all the metrics on the experiment (See [Removing a Metric from a Flag Experiment](projects.md#removing-a-metric-from-a-flag-experiment)).