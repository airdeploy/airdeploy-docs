---
id: environments
title: Environments
sidebar_label: Environments
---

Each project contains environments. Although all flags exist at the project level, user and entity data is partitioned by environment. Environments essentially allow you to toggle flags separately in different environments. For example, a flag may be on and fully rolled out in a "Staging" environment, but not yet active in a "Production" environment.

Each project is created with two environments - a "Production" and "Test" environment. You can add and remove environments as you see fit.

Each environment has two API keys: there is a secret API key, meant for use with backend SDKs, as well as a publishable one for use for in-browser applications.

## Switching Environment Views

Anywhere you see the Environment Switcher in Airdeploy it means the screen is context-sensitive to that environment. It also means you can switch environments to work with the content in that environment.

![](assets/switching-env.png)

- Click the Environment Switcher.
- Choose an environment from the list of environments.

## Managing Environments

To manage project and environments, find `Projects` in the main navigation menu under your organization.

## Adding an Environment

- Find the project for which you wish to add the environment
- Click the “Add an Environment” button at the end of the environment list for that project. A side panel appears
- Complete the sidebar form and save

> Please note that only Admins and Owners can create environments

## Deleting an Environment

`Main Navigation, Organization menu > Choose Projects & Environments`

You cannot delete the Prod and Dev environments.

- Hover over the environment card
- Click the "Delete Environment" button
- Confirm the dialog warning to continue

> Please note that only Admins and Owners can delete environments
