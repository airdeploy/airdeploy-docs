---
id: version-3.0.x-projects
title: Projects
sidebar_label: Projects
original_id: projects
---

## Managing Projects

`Main Navigation > Projects & Environments`

This screen enables you to add or remove projects, as well as environments (see [Environments](environments.md)).

![](assets/managing-projects.png)

### Add a Project 

- Click the “+” to add a project
- Complete the sidebar form information and click “Save”

### Delete a Project

- Hover over the project name
- Click the “Delete Project” button in the section divider
- Confirm the dialog warning to continue, or cancel to stop.

### Open a Project
There are two ways to open a project:

- From the Projects & Environments screen
    - Click on the project name
- From the Left Navigation
    - Click on the current project name
    - Choose a project from the dropdown


## Managing Flags

`Main Navigation > Choose a Project from the dropdown`

This screen displays all flags for this project. They are organized by flags you created or are a member of (“My Flags”), “Other Flags,” and “Archived.”

![](assets/managing-flags.png)

### Add a Flag

- Click the “+” to add a flag
- Complete the sidebar form information and click “Save”

### Open a Flag

- Click the flag name

### Enable and Disable a Flag
When a flag is disabled, all entities will not see any variations. When a flag is enabled, the configured entity subsets will see their assigned variations.

- Click the toggle under a flag to enable or disable it.
- The Flag is immediately enabled or disabled. (is this still valid? We pack changes rn AFAIK Alvin to review)

### Archive a Flag
Archiving a flag is a great way to declutter you “My Flags” portion of the Projects screen while retaining the flag’s configuration. 

- Open a Flag
- Click the “Archive Flag” in the top bar
- Confirm the dialog warning to continue, or cancel to stop

### Delete a Flag
Deleting a flag is a permanent decision. All flag configuration is removed, include audit trails and team members. Consider Archiving before deleting.

- Open a Flag
- Click the trash can icon (“Delete”) in the top bar
- Confirm the dialog warning to continue, or cancel to stop


## Understanding Flag Activity

Airdeploy displays a rolling 30-day activity summary for each flag in the project screen.

![](assets/flag-activity.png)

12am to 5:59am, 6:00am to 11:59am, 12pm to 5:59pm, 6:00pm to 11:59pm). The columns run from left as the oldest (30-days ago) to the right as the newest (the last 24-hours). The rightmost column is blue in order to call it out glanceably.

Each square has five levels of activity. “Off” is default. The four other levels are relative to the overall activity of the flag traffic \[need details]

## Metrics

Metrics are used for running experiments on project Flags (see [Experiments](flags.md#flag-experiments)). 

Metrics are global to a Project. You can create them in two locations—at the Project level, or on a Flag Experiment. Removing a metric from a project

### Creating a Metric for the Project

`Main Navigation > A Project Page > Metrics`
 
- Click the "+”. A side panel appears.
- In the side panel, complete the fields.
- Click “Add Metric”
> Note: Creating a metric does not create an experiment. Metrics are available to all Flag Experiments within a Project.

### Deleting a Metric from the Project

- Hover over the Metric you wish to delete.
- Click the “X” which appears.
- Confirm the dialog warning to continue, or cancel to stop.
>Note: Deleting a Metric from the Project removes all historical data collected on that metric for every Flag with an Experiment using it.

### Creating a Metric on a Flag Experiment

- Click “Add a Metric” in the top menu of Experiments. A sidebar appears.
- Choose whether to Choose an Existing Metric, or Create a New Metric.
    - Choose an Existing Metric
        - Select the metric from the dropdown.
        - Click “Use This Metric”
    - Create a New Metric
        - In the side panel, complete the fields.
        - Click “Add Metric”
>Note: This metric is now available to all Flag Experiments in this Project, but only active on the Flag Experiment you are in when you create it.

### Removing a Metric from a Flag Experiment

- Hover over the metric you wish to remove. 
- Click on the “X” tab that appears.
- Confirm the dialog warning to continue, or cancel to stop.
> Note: Deleting a metric from a flag removes all historical data collected on that metric for this flag. If you add the same metric back, data will be collected starting from that date.
    
> Note: the metric is not deleted from the Project.
