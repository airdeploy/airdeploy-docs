---
id: overview
title: Overview
sidebar_label: Overview
---

The Flagger SDKs are a set of open-source libraries that allow you to integrate feature flags into your application. It can connect to a remote source (like Airdeploy) so flags can be updated in real-time.

Each Flagger SDK implements a common API, and implements feature flags according to a shared approach.

### Deterministic & Consistent

For any given entity, a feature flag should resolve in an identical way - across languages, devices, operating systems, etc. It also means the flag should resolve consistently. Meaning users should not flip-flop between different states or variations.

### Local / Stateful

Resolution of a feature flag should happen locally - without a depdendency on a constant network connection. That means Flagger SDKs are stateful, that - although relies on a remotely retrieved configuration - does not rely on a network connection to resolve flags.

### Updates in Real-Time

Flagger receives lives and atomic updates of changes made to feature flags. Applications integrated with Flagger do not need to be restarted to receive the updated configuration. As soon as changes are made remotely (e.g. through the Airdeploy dashboard), they should be reflected on all instances of the application with Flagger installed and initialized.

### Data Ingestion

Flagger returns data to it's remote server that is useful, but does appropriate batching and caching to decrease network usage.

## Supported Languages

Currently, Flagger supports the following languages:

- Javascript / Node.js
- React
- Java
- Ruby
- Python
- Golang
- Swift(iOS)
