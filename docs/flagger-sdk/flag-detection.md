---
id: flag-detection
title: Flag Detection
sidebar_label: Flag Detection
---

Flagger automatically detects any new flags in the code and sends it to Airship via ingestion mechanism.

As you may already know, `Flagger` fetch `FlaggerConfiguration` from Airship. This configuration is required for 
`Flagger` to work. If you use a codename that is not in the current `FlaggerConfiguration`, `Flagger` would detect it and 
would notify Airship in the next ingestion request.
 
There is an example of that at [initialization section](quick-start.md#make-a-test-flag-request). There we create a 
flag that currently is neither in Airship nor in Flagger, but due to the Flag Detection we can see it in the Airship Dashboard.
