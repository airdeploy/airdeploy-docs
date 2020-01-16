---
id: overview
title: Overview
sidebar_label: Overview
---

Flagger is the open-source implementation of the feature flagging(feature gating, feature toggles) concept. 
It works both in the browser and on the server. 

#### Client libraries:
- Nodejs
- React
- Java 
- Python
- Ruby
- Go

## Features
- Blazing fast does not require a backend call to decide what flag to show
- Highly customizable
- Sampling(canary release) and multivariate A/B testing with custom filters
- Auto-updatable configuration(via SSE)
- Records usage analytics as well as custom events
- white- and blacklisting
- 2 level entity support (Manager-Employee, Client-Company)

High level speaking Flagger contains two different parts: a client library and a server.
There are two options for the server: self-hosted community version and Airship-hosted Professional version.

## Design Principles
Description: I'm trying to have each docs section have a "Design Principles" section. Let's see if it fits.

## Flags
Description: Show how to use Flagger to set up different types of flags. 
Simple on/off flags, multivariate experiments, etc. 
How to use different functions / methods.
.getVariation()
.isEnabled()
.isSampled()
.getPayload()

##### Default variation:
```json
{
	"isEnabled": false,
	"isSampled": false,
	"variation": "off",
	"payload": {}
}
```

## Flag Detection
Flagger will automatically detect new flags in the code and sends them to Airship via ingestion mechanism. 
You can see an example of that at [initialization section](installation.md#make-a-test-flag-request), when we create a flag that currently does not exist in 
Airship, but due to the flag detection we can see it after running code example.

## Order of Precedence
Description: How to use Flagger to track events/analytics

## API Reference
Description: Full API reference
