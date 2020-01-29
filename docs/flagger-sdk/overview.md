---
id: overview
title: Overview
sidebar_label: Overview
---

Flagger is the open-source implementation of the feature flagging(feature gating, feature toggles) concept. 

## Features
- Blazing fast, requires only one backend call
- Highly customizable
- Sampling(canary release) and multivariate A/B testing with custom filters
- Auto-updatable configuration(via SSE)
- Records usage analytics as well as custom events
- white- and blacklisting
- 2 level entity support (Manager-Employee, Client-Company)


## Design Principles
Description: I'm trying to have each docs section have a "Design Principles" section. Let's see if it fits.

## Flag Detection
Flagger will automatically detect new flags in the code and sends them to Airship via ingestion mechanism. 
You can see an example of that at [initialization section](installation.md#make-a-test-flag-request), when we create a 
flag that currently does not exist in Airship, but due to the flag detection we can see it after running code example.

## Order of Precedence
Order of Precedence

The final combined order of precedence for determining treatments is:

1. Kill Switch: Off if the flag is "killed" (kill switch engaged)
2. Individual Blacklist: Off if individual entity is blacklisted.
3. Individual Whitelist: On if individual entity is whitelisted. If the flag is multivariate, the specified treatment is served.
4. Group Blacklist: Off if enclosing group entity is blacklisted.
5. Group Whitelist: On if enclosing group entity is whitelisted. If the flag is multivariate, the specified treatment is served.
6. Individual Population Sampled: On if individual entity is in a sampled population. If the flag is multivariate, a treatment is randomly assigned based on the allocation provided.
7. Group Population Sampled: On if enclosing group entity is in a sampled population. If the flag is multivariate, a treatment is randomly assigned based on the allocation provided.
8. Default Variation