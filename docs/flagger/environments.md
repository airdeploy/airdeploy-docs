---
id: environments
title: Environments
sidebar_label: Environments
---

Each project contains at least two environments—Prod and Dev. You can add as many environments as you need. Each project’s environments are specific to that project. So each project can contain a different number of environments.

Flags are specific to an environment. This means that a Flag named “Billing” in Prod is a different from a Flag named the same in Dev. Switching between environments is easy throughout Airship, but it is am important point to note.

Each environment has two keys. The ENV Key is Alvin. The ENV Key for Browsers is Alvin.

Entities are also specific to environments.  

## Switching Environment Views

Anywhere you see the Environment Switcher in Airship it means the screen is context-sensitive to that environment. It also means you can switch environments to work with the content in that environment.

![](assets/switching-env.png)

- Click the Environment Switcher.
- Choose an environment from the list of environments.

## Adding an Environment

`Main Navigation, Organization menu > Choose Projects & Environments`

- Find the project section you wish to add the environment.
- Click the “Add an Environment” button at the end of the environment list for that project. A side panel appears.
- Complete the environment name.
- Click “Save Environment”

## Deleting an Environment

`Main Navigation, Organization menu > Choose Projects & Environments`

You cannot delete the Prod and Dev environments.


- Find the project section you wish to remove the environment.
- Click the “Delete Environment” button on the environment you wish to remove.
- Confirm the dialog warning to continue, or cancel to stop
    - > Note: Deleting an environment removes all keys, entities, and flags associated with that environment from Airship.