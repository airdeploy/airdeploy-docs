---
id: php
title: PHP API Reference
sidebar_label: PHP
---

## Flagger

### init

```
public static function init($config)
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `init` must be called only once, at the start of your application.
> Your program **must** wait for `init` to finish before using any other `Flagger` methods

```
$apiKey = "<API-KEY>"; # could be omitted if FLAGGER_API_KEY env variable is set
$logLevel = "debug"; # could be omitted, 'error' by default
Flagger::init(['apiKey' => $apiKey, 'logLevel' => $logLevel]);
```

| name         | Environment variable      | Default                                     | Description                                                                                             |
| ------------ | ------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey       | FLAGGER_API_KEY           | None                                        | API key to an environment                                                                               |
| sourceURL    | FLAGGER_SOURCE_URL        | https://flags.airdeploy.io/v3/config/       | URL to get `FlaggerConfiguration`                                                                       |
| backupURL    | FLAGGER_BACKUP_SOURCE_URL | https://backup-api.airshiphq.com/v3/config/ | backup URL to get `FlaggerConfiguration`                                                                |
| sseURL       | FLAGGER_SSE_URL           | https://sse.airdeploy.io/v3/sse/            | URL for real-time updates of `FlaggerConfiguration` via sse                                             |
| ingestionURL | FLAGGER_INGESTION_URL     | https://ingestion.airdeploy.io/v3/ingest/   | URL for ingestion                                                                                       |
| logLevel     | FLAGGER_LOG_LEVEL         | ERROR                                       | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

- If not provided default arguments values are used and printed to Debug
- If second(third …) call of `init` happens:
  - If the arguments are the same, `init` method does nothing
  - If arguments differ, `Flagger` prints warnings and recreates(closes and creates new) resources(SSE connection,
    Ingester, gets new `FlaggerConfiguration`).
  - > Note: you must call init only once
- If initial `FlaggerConfiguration` is not fetched from source/backup, Flagger prints a warning
- If `Flagger` fails to get `FlaggerConfiguration` then all Flags Functions return [Default Variation](../flagger-sdk/default-variation.md)
- If Flagger fails to establish SSE connection, it retries every 30 seconds until succeeded
- If you call any Flag Function BEFORE `init` is finished then you'll get [Default Variation](../flagger-sdk/default-variation.md)

### shutdown

```
public static function shutdown(int $timeoutMS): bool
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`shutdown` waits until current ingestion request is finished, but no longer than a `timeout`.

returns `true` if closed by timeout

> Note: you **must** call shutdown only once before the end of the application runtime.

```
Flagger::shutdown(1000);
```

### publish

```
public static function publish($entity)
```

Explicitly notifies Airdeploy about an Entity

```
Flagger::publish(['id' => "42"]);
```

### track

```
public static function track($eventName, $eventProperties, $entity = null)
```

Event tracking API.
Entity is an optional parameter if it was set before.

```
Flagger::track('test', ['age' => 40], ['id' => '42']);
```

### setEntity

```
public static function setEntity($entity = null)
```

Stores an entity in Flagger, which allows omission of entity in other API methods.

```
Flagger::setEntity(['id' => '42']);

# here we are omitting, because Flagger has already stored "entity"
Flagger::isEnabled('color-theme');
```

> If you don't provide **any** entity to Flagger:
>
> - flag functions always resolve with the default variation
> - `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

### isEnabled

```
public static function isEnabled(string $codename, $entity = null): bool
```

Determines if flag is enabled for entity.

```
$isEnabled = Flagger::isEnabled('color-theme', ['id' => '42']);
```

Group example:

```
$isEnabled = Flagger::isEnabled('color-theme', ['id' =>"42", 'group' => ['id' => "4242", 'type' => "company"]]);
```

### isSampled

```
public static function isSampled(string $codename, $entity = null): bool
```

Determines if entity is within the targeted subpopulations

```
$isSampled = Flagger::isSampled('color-theme', ['id' => '42']);
```

Group example:

```
$isSampled = Flagger::isSampled('color-theme', ['id' => "42", 'group' => ['id' => "4242", 'type' => "company"]]);
```

### getVariation

```
public static function getVariation(string $codename, $entity = null): string
```

Returns the variation assigned to the entity in a multivariate flag

```
$variation = Flagger::getVariation("color-theme", ['id' => '42']);
```

Group example:

```
$variation = Flagger::getVariation('color-theme', ['id' => "42", 'group' => ['id' => "4242", 'type' => "company"]]);
```

### getPayload

```
public static function getPayload(string $codename, $entity = null)
```

Returns the payload associated with the treatment assigned to the entity

```
$payload = Flagger::getPayload('color-theme', ['id' => '42']);
```

Group example:

```
$payload = Flagger::getPayload('color-theme', ['id' => "42", 'group' => ['id' => "4242", 'type' => "company"]]);
```
