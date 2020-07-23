---
id: introduction
title: Introduction
sidebar_label: Introduction
---

## Flags, Switches, Toggles

Flags allow you to insert a point in your code where traffic can be routed (or "switched") from one path to another.

They are extremely useful for developing and launching new features. Let’s say you are developing an accounting app with a Billing feature that generates invoices monthly. You now want to try a new Billing feature that generates invoices weekly.

Traditionaly, developers cut a new feature branch and work on that new feature separately until it's complete, tested, and ready to go-live. Then you deploy the code to "launch" the feature. This is the most rigid approach because it requires the most upfront QA and testing, and looks like this:

![users](assets/users.jpeg)

A different approach would be to create a code switch to split traffic based on some parameters, looking something like this:

![code-switch](assets/code-switch.jpeg)

This brings the new feature into the existing codebase, where it can be brought up to full traffic gradually. However, if the code switch is static, it can only be updated by re-deploying or re-shipping code. This is not always an expedient, quick way to update who sees a new feature.

Airdeploy allows your code switches to be based on dynamic parameters. Basically, this is what we call a "flag" - a code switch that Airdeploy can remotely control. Flags allow you to grant access to features dynamically at runtime, separately from your code deployment process.

You can configure flags to:

- Turn your feature on or off without redeploying
- Roll out new features
- Manage customer feature permissions
- Add variations to your feature
- Rebalance traffic of your users or entities to variations of the feature
- Direct specific individuals or entities to specific variations
- Preview how your traffic population will flow through your switch point as you configure your flag
- Run experiments or A/B tests

## Beyond Toggles

We've looked at a simple flag - essentially an on/off switch for a feature. But flags in Airdeploy can do quite a bit more. For example:

![Flag On](assets/variation-flag-on.jpeg)
In this example, a feature is not simply "on" or "off". There are multiple variations that the traffic can be assigned to. Each of the three variations are equally likely in this case.

![Flag Off](assets/variation-flag-off.jpeg)
This flag is switched off, so the flag simply routes traffic back into the normal codepath. Additionally, only a subpopulation of the entity traffic is diverted to the flag - in this case based on the operating system of the user. The rest of the traffic continues along the default codepath.

You can model more advanced scenarios and combinations in Airdeploy's dashboard. A few examples:

#### Subpopulations

When filtering traffic that will pass through the flag, you can limit the traffic flow by adding filters and by dialing up / down the sampling percentage.

![Entity Subpopulation](assets/entity-subset.jpeg)
In this case, 80% of all users are being diverted to the flag.

#### Whitelisting

You can also assign any entity to a specific variation and they will see that variation (as long as the flag is on). Whitelists bypass all the filters, criteria, and randomization.

![Whitelisting](assets/wl-bl.jpeg)
