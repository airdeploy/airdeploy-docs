---
id: data
title: Data Destinations & Events
sidebar_label: Data Destinations & Events
---

`Main Navigation > Choose a Project > Choose Data Destinations`


![](assets/data-destinations.png)


Every interaction with Entities and Flags creates an Event. There are two types of events:

- __Assignment Events__ — Logged whenever an Entity is assigned to a Feature variation 
- __User Events__ — Logged whenever a Entity Type of User takes an action.

Events are specific to environment, so there is an Environment Switcher available.

## Recent Event Stream

Events are continuously logged. You can view the stream by Assignment Events or user Events. Each event carries detail information.

- Type
- ID
- Name
- The event description
    - “{project Name} {Feature variation}” for Assignment Events
    - “clicked on {…}” for User Events
- Date and Time stamp

### Event Preview
Clicking on any event displays a panel with a JSON file detailing the event.

## Destinations

You can capture this event stream and direct the data to almost any external URL. By default, the event stream is not directed anywhere.

### Adding Destinations

- Click the “+”. A side panel will appear.
- Complete the Destination information
    - > Note: You have the chance to test the URL
    
    - > Note: You have the choice to add a Destination in a deactivated state.
- Click “Save Destination”

### Enabling/Disabling Destinations

- Locate the Destination in the list
- Toggle the switch to enable or disable the Destination.
- The change will be immediate.

### Modifying or Deleting Destinations

- Locate the Destination in the list
- Hover over the Destination. Functions will appear.
    - Click the right arrow to edit
    - Click the “X” to delete
        - Confirm the dialog warning to continue, or cancel to stop.