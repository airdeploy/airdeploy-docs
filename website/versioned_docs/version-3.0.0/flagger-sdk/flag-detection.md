---
id: version-3.0.0-flag-detection
title: Flag Detection
sidebar_label: Flag Detection
original_id: flag-detection
---

Flagger automatically detects any new flags in the code and sends it to Airdeploy via ingestion mechanism.

As you may already know, `Flagger` fetches `FlaggerConfiguration` from Airdeploy. This configuration is required for 
`Flagger` to work. If you use a codename that is not in the current `FlaggerConfiguration`, `Flagger` detects it and 
notifies Airdeploy about it.
 
There is an example of that at [initialization section](quick-start.md#make-a-test-flag-request). There, we create a 
flag that currently is neither in Airdeploy nor in Flagger, but due to the Flag Detection we can see it's in the 
Airdeploy Dashboard.
