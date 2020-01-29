---
id: api-reference-ts
title: TypeScript API Reference
sidebar_label: Typescript
---

## Flagger
### Flagger.init(options): Promise<FlaggerInstance>

`init` gets `FlaggerConfiguration`, establish SSE connections and starts an ingester

> `init` must be called only once, at the start of your application. Your program should wait for the promise to resolve so flag functions works properly

```javascript
import Flagger from 'flagger'

await Flagger.init({
        "apiKey": "k4k3llrkfl2234l", // the only required option
        "sourceURL": "flagger.notairshiphq.com",
        "sourceBackupURL": "backupFlagger.notairshiphq.com",
        "sseURL": "sse.notairshiphq.com",
        "ingestionURL": "ingestion.notairshiphq.com",
        "logLevel": 'DEBUG'
})
```

| name            | type   | Required | Default                           | Description                                                                                             |
| --------------- | ------ | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey          | string | true     | None                              | API key to an environment                                                                               |
| sourceUrl       | string | false    | https://api.airshiphq.com/        | URL to get FlaggerConfiguration                                                                         |
| backupSourceUrl | string | false    | https://backup-api.airshiphq.com/ | backup URL to get FlaggerConfiguration                                                                  |
| sseUrl          | string | false    | https://sse.airshiphq.com/        | URL for real-time updates via sse                                                                       |
| ingestionUrl    | string | false    | https://ingestion.airshiphq.com   | URL for ingestion                                                                                       |
| logLevel        | string | false    | ERROR                             | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

Flagger init function specs
- If `apiKey` is not provided `init` function throws(or returns) an error
- Optional values are propagated with defaults and print to Debug
- if second(third …) call of `init` happens:
    - if the arguments are the same, `init` method does nothing
    - if arguments differ, Flagger Warns about it and recreates(closes and creates new) resources(SSE connection, 
    Ingester, gets new FlaggerConfiguration).
- if initial `FlaggerConfiguration` is not fetched from source/backup than print error as Warning
- if SSE connection fails than print error as Warning and retry until connection is established
- if source/backup/SSE fail to get `FlaggerConfiguration` then Flagger is still working, but all flags functions return 
default variation
- any flag function that is called BEFORE the Init finishes returns default variation  


### Flagger.addFlaggerConfigUpdateListener(listener: (config: FlaggerConfiguration) ⇒ void): void

Add a listener to subscribe to event when `Flagger` gets new `FlaggerConfiguration`
### Flagger.removeFlaggerConfigUdateListener(listener: (config: FlaggerConfiguration)⇒ void): void

Removes a listener

### Flagger.publish(entity: Entity): void

Sends an entity explicitly to Airship

```javascript
Flagger.publish({id:1})
```


### Flagger.track(eventName: String, eventProperties: Object, entity: Entity): void

Simple event tracking API.
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

### Flagger.setEntity(entity: Entity): void

`setEntity` stores an entity in Flagger, which allows omission of entity in other API methods. 
Can be called to replace default entity.

```javascript
const entity = {
    "id": 543,
    "type": "User", // optional - type defaults to "User"
    "name": "John Smithson"
}
Flagger.setEntity(entity)

// here we are omitting, because Flagger has stored "entity"
Flagger.flagIsEnabled("stripe-payment")  
Flagger.track("some-event", {content: "some-text"})

// Flagger will use "differentEntity", because it will override "entity"
const differentEntity = {id:"94239643"}
Flagger.flagIsEnabled("strip-payment", differentEntity) 

Flagger.setEntity(null) // to remove global entity
```

>If you don't provide an entity to Flagger:
>- flag functions always resolve with the default variation
>- track doesn't record an event

Rule of thumb: make sure you provided an entity to the Flagger

## Flag functions
### Flagger.flagIsEnabled(codename: String, entity: Entity): Boolean

Determines if flag is enabled for entity.

    const isEnabled = Flagger.flagIsEnabled("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const isEnabled = Flagger.flagisEnabled("bitcoin-pay", entityWithGroup);



### Flagger.flagIsSampled(codename: String, entity: Entity): Boolean

Determines if entity is within the targeted subpopulations

    const isSampled = Flagger.flagIsSampled("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const isSampled = Flagger.flagIsSampled("bitcoin-pay", entityWithGroup);


### Flagger.flagGetVariation(codename: String, entity: Entity): String

Returns the variation assigned to the entity in a multivariate flag

    const variation = Flagger.flagGetVariation("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const variation = Flagger.flagGetVariation("bitcoin-pay", entityWithGroup);



### Flagger.flagGetPayload(codename: String, entity: Entity): Object

Returnsthe payload associated with the treatment assigned to the entity

    const payload = Flagger.flagGetPayload("bitcoin-pay", { id: 1 });

Group example:

    const entityWithGroup = {
        type: 'User',
        id: '19421826',
        group: {type: 'Company', id: '52272353'}
      }
    const payload = Flagger.flagGetPayload("bitcoin-pay", entityWithGroup);