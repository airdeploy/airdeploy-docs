---
id: js
title: Javascript API Reference
sidebar_label: Javascript
---

## Flagger

### init

```javascript
Flagger.init(options): Promise<void>
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `init` must be called only once, at the start of your application.
> Your program **must** wait for the promise to resolve before using any other `Flagger` methods

```javascript
import Flagger from 'flagger'

await Flagger.init({
  apiKey: '<API-KEY>', // the only required field
  logLevel: 'DEBUG',
})
```

| name            | type   | Required | Default                                     | Description                                                                                             |
| --------------- | ------ | -------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey          | string | true     | None                                        | API key to an environment                                                                               |
| sourceURL       | string | false    | https://flags.airdeploy.io/v3/config/       | URL to get `FlaggerConfiguration`                                                                       |
| backupSourceURL | string | false    | https://backup-api.airshiphq.com/v3/config/ | backup URL to get `FlaggerConfiguration`                                                                |
| sseURL          | string | false    | https://sse.airdeploy.io/v3/sse/            | URL for real-time updates of `FlaggerConfiguration` via sse                                             |
| ingestionURL    | string | false    | https://ingestion.airdeploy.io/v3/ingest/   | URL for ingestion                                                                                       |
| logLevel        | string | false    | ERROR                                       | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

- If `apiKey` is not provided `init` promise is rejected
- If not provided default arguments values are used and printed to Debug
- If second(third …) call of `init` happens:
  - If the arguments are the same, `init` method does nothing
  - If arguments differ, `Flagger` prints warnings and recreates(closes and creates new) resources(SSE connection,
    Ingester, gets new `FlaggerConfiguration`).
  - > Note: you must call init only once
- If initial `FlaggerConfiguration` is not fetched from source/backup, Flagger prints a warning
- If `Flagger` fails to get `FlaggerConfiguration` then all Flags Functions return [Default Variation](../flagger-sdk/default-variation.md)
- If Flagger fails to establish SSE connection, it retries every 30 seconds until succeeded
- If you call any Flag Function BEFORE `init` promise is resolved then you get [Default Variation](../flagger-sdk/default-variation.md)

### shutdown

```javascript
Flagger.shutdown(): Promise<void>
```

ingests data(if any), stops ingester and closes SSE connection.

> Note: you **must** call shutdown only once before the end of the application runtime.

```javascript
await Flagger.shutdown()
```

### addFlaggerConfigUpdateListener

```javascript
public static addFlaggerConfigUpdateListener(
    listener: (config: IFlaggerConfiguration) => void
  ): void
```

Add a listener to subscribe to event when `Flagger` gets new `FlaggerConfiguration`

### removeFlaggerConfigUdateListener

```javascript
public static removeFlaggerConfigUpdateListener(
    listener: (config: IFlaggerConfiguration) => void
  ): void
```

Removes a listener

### publish

```javascript
Flagger.publish(entity: Entity): void
```

Explicitly notify Airdeploy about an Entity

```javascript
Flagger.publish({id: 1})
```

### track

```javascript
Flagger.track(eventName: String, eventProperties: Object, entity: Entity): void
```

Event tracking API.
Entity is an optional parameter if it was set before.

```javascript
Flagger.track(
  'Purchase Completed',
  {
    plan: 'Gold',
    referrer: 'www.Google.com',
    shirt_size: 'medium',
  },
  {id: 543}
)

// If `entity` has been set before:
Flagger.track('Purchase Completed', {
  plan: 'Gold',
  referrer: 'www.Google.com',
  shirt_size: 'medium',
})
```

### setEntity

```javascript
Flagger.setEntity(entity: Entity): void
```

`setEntity` stores an entity in Flagger, which allows omission of entity in other API methods.

```javascript
const entity = {
  id: 543,
  type: 'User', // optional - type defaults to "User"
  name: 'John Smithson',
}
Flagger.setEntity(entity)

// here we are omitting, because Flagger has already stored "entity"
Flagger.isEnabled('stripe-payment')
Flagger.track('some-event', {content: 'some-text'})

// Flagger will use "differentEntity", because it will override "entity"
const differentEntity = {id: '94239643'}
Flagger.isEnabled('strip-payment', differentEntity)

Flagger.setEntity(null) // to remove global entity
```

> If you don't provide **any** entity to Flagger:
>
> - flag functions always resolve with the default variation
> - `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

### isEnabled

```javascript
Flagger.isEnabled(codename: String, entity: Entity): Boolean
```

Determines if flag is enabled for entity.

```javascript
const isEnabled = Flagger.isEnabled('color-theme', {id: 1})
```

Group example:

```javascript
const entityWithGroup = {
  type: 'User',
  id: '19421826',
  group: {type: 'Company', id: '52272353'},
}
const isEnabled = Flagger.flagisEnabled('color-theme', entityWithGroup)
```

### isSampled

```javascript
Flagger.isSampled(codename: String, entity: Entity): Boolean
```

Determines if entity is within the targeted subpopulations

```javascript
const isSampled = Flagger.isSampled('color-theme', {id: 1})
```

Group example:

```javascript
const entityWithGroup = {
  type: 'User',
  id: '19421826',
  group: {type: 'Company', id: '52272353'},
}
const isSampled = Flagger.isSampled('color-theme', entityWithGroup)
```

### getVariation

```javascript
Flagger.getVariation(codename: String, entity: Entity): String
```

Returns the variation assigned to the entity in a multivariate flag

```javascript
const variation = Flagger.getVariation('color-theme', {id: 1})
```

Group example:

```javascript
const entityWithGroup = {
  type: 'User',
  id: '19421826',
  group: {type: 'Company', id: '52272353'},
}
const variation = Flagger.getVariation('color-theme', entityWithGroup)
```

### getPayload

```javascript
Flagger.getPayload(codename: String, entity: Entity): Object
```

Returns the payload associated with the treatment assigned to the entity

```javascript
const payload = Flagger.getPayload('color-theme', {id: 1})
```

Group example:

```javascript
const entityWithGroup = {
  type: 'User',
  id: '19421826',
  group: {type: 'Company', id: '52272353'},
}
const payload = Flagger.getPayload('color-theme', entityWithGroup)
```
