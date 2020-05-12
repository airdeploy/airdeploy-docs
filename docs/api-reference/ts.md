---
id: ts
title: TypeScript API Reference
sidebar_label: Typescript
---

## Flagger
### init
```typescript
Flagger.init(options): Promise<FlaggerInstance>
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initialize Ingester

> Note: `init` must be called only once, at the start of your application. 
>Your program __must__ wait for the promise to resolve before using any other `Flagger` methods

```javascript
import Flagger from 'flagger'

await Flagger.init({
        "apiKey": "k4k3llrkfl2234l", // the only required option
        "sourceURL": "https://flagger.notairdeploy.io",
        "backupSourceURL": "https://backupflagger.notairdeploy.io",
        "sseURL": "https://sse.notairdeploy.io",
        "ingestionURL": "https://ingestion.notairdeploy.io",
        "logLevel": 'DEBUG'
})
```

| name            | type   | Required | Default                           | Description                                                                                             |
| --------------- | ------ | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey          | string | true     | None                              | API key to an environment                                                                               |
| sourceURL       | string | false    | https://api.airdeploy.io/configurations/        | URL to get `FlaggerConfiguration`                                                                         |
| backupSourceURL | string | false    | https://backup-api.airdeploy.io/configurations/ | backup URL to get `FlaggerConfiguration`                                                                  |
| sseURL          | string | false    | https://sse.airdeploy.io/sse/v3/?envKey=        | URL for real-time updates of `FlaggerConfiguration` via sse                                                                       |
| ingestionURL    | string | false    | https://ingestion.airdeploy.io/collector?envKey=   | URL for ingestion                                                                                       |
| logLevel        | string | false    | ERROR                             | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

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

```typescript
Flagger.shutdown(): Promise<void>
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.

> Note: you __must__ call shutdown only once before the end of the application runtime. 

```typescript
await Flagger.shutdown()
```

### addFlaggerConfigUpdateListener

```typescript
Flagger.addFlaggerConfigUpdateListener(listener: (config: FlaggerConfiguration) ⇒ void): void
```

Add a listener to subscribe to event when `Flagger` gets new `FlaggerConfiguration`

### removeFlaggerConfigUdateListener

```typescript
Flagger.removeFlaggerConfigUpdateListener(listener: (config: FlaggerConfiguration)⇒ void): void
```

Removes a listener

### publish

```typescript
Flagger.publish(entity: Entity): void
```

Explicitly notify Airdeploy about an Entity

```javascript
Flagger.publish({id:1})
```


### track

```typescript
Flagger.track(eventName: String, eventProperties: Object, entity: Entity): void
```

Event tracking API.
Entity is an optional parameter if it was set before.

```javascript
Flagger.track('Purchase Completed', {
        "plan": "Gold",
  "referrer": "www.Google.com",
  "shirt_size": "medium"
}, {id: 543})

// If `entity` has been set before:
Flagger.track('Purchase Completed', {
        "plan": "Gold",
  "referrer": "www.Google.com",
  "shirt_size": "medium"
})
```

### setEntity

```typescript
Flagger.setEntity(entity: Entity): void
```

`setEntity` stores an entity in Flagger, which allows omission of entity in other API methods. 

```javascript
const entity = {
    "id": 543,
    "type": "User", // optional - type defaults to "User"
    "name": "John Smithson"
}
Flagger.setEntity(entity)

// here we are omitting, because Flagger has already stored "entity"
Flagger.flagIsEnabled("stripe-payment")  
Flagger.track("some-event", {content: "some-text"})

// Flagger will use "differentEntity", because it will override "entity"
const differentEntity = {id:"94239643"}
Flagger.flagIsEnabled("strip-payment", differentEntity) 

Flagger.setEntity(null) // to remove global entity
```

>If you don't provide __any__ entity to Flagger:
>- flag functions always resolve with the default variation
>- `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

## Flag Functions
### flagIsEnabled

```typescript
Flagger.flagIsEnabled(codename: String, entity: Entity): Boolean
```

Determines if flag is enabled for entity.

    const isEnabled = Flagger.flagIsEnabled("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const isEnabled = Flagger.flagisEnabled("bitcoin-pay", entityWithGroup);



### flagIsSampled

```typescript
Flagger.flagIsSampled(codename: String, entity: Entity): Boolean
```

Determines if entity is within the targeted subpopulations

    const isSampled = Flagger.flagIsSampled("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const isSampled = Flagger.flagIsSampled("bitcoin-pay", entityWithGroup);


### flagGetVariation

```typescript
Flagger.flagGetVariation(codename: String, entity: Entity): String
```

Returns the variation assigned to the entity in a multivariate flag

    const variation = Flagger.flagGetVariation("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const variation = Flagger.flagGetVariation("bitcoin-pay", entityWithGroup);



### flagGetPayload

```typescript
Flagger.flagGetPayload(codename: String, entity: Entity): Object
```

Returns the payload associated with the treatment assigned to the entity

    const payload = Flagger.flagGetPayload("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const payload = Flagger.flagGetPayload("bitcoin-pay", entityWithGroup);