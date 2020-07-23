---
id: flag-detection
title: Flag Detection
sidebar_label: Flag Detection
---

Flagger automatically detects any new flags in your application code and sends it to Airdeploy dashboard. Flags can be created in code without first going to the dashboard, and configured later. By default, the flag is considered to be "off" until configured otherwise.

There is an example of this in the [quickstart](quick-start.md#make-a-test-flag-request). There, we create a flag new flag just by asking Flagger to resolve it in code. It will automatically show up in the Airdeploy dashboard after the next time the application executes that Flagger call.
