---
id: version-3.0.x-java
title: Java API Reference
sidebar_label: Java
original_id: java
---

## Flagger
### init

```java
public static void init(FlaggerInitConfig config)
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `init` must be called only once, at the start of your application. 
>Your program __must__ wait for `init` to finish before using any other `Flagger` methods

```java
import io.airdeploy.flagger.*;

public class Main {

    public static void main(String[] args) {
        String apiKey = "x2ftC7QtG7arQW9l";
        FlaggerInitConfig flaggerInitConfig = FlaggerInitConfig.builder()
                .apiKey(apiKey) // the only required field
                .logLevel(LogLevel.DEBUG)
                .build();
        Flagger.init(flaggerInitConfig);


        // rest of the app code goes here
        boolean enabled = Flagger.flagIsEnabled("group-messaging", Entity.builder().id("57145770").build());
       
        System.out.println(enabled);
    }
}
```

| name            | type   | Required | Default                           | Description                                                                                             |
| --------------- | ------ | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey          | string | true     | None                              | API key to an environment                                                                               |
| sourceUrl       | string | false    | https://api.airdeploy.io/configurations/        | URL to get `FlaggerConfiguration`                                                                         |
| backupSourceURL | string | false    | https://backup-api.airdeploy.io/configurations/ | backup URL to get `FlaggerConfiguration`                                                                  |
| sseUrl          | string | false    | https://sse.airdeploy.io/sse/v3/?envKey=        | URL for real-time updates of `FlaggerConfiguration` via sse                                                                       |
| ingestionUrl    | string | false    | https://ingestion.airdeploy.io/collector?envKey=   | URL for ingestion                                                                                       |
| logLevel        | string | false    | ERROR                             | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

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

```java
public static boolean shutdown(int timeoutMillis) 
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`shutdown` waits until current ingestion request is finished, but no longer than a timeoutMillis.

returns `true` if closed by timeout 

> Note: you __must__ call shutdown only once before the end of the application runtime. 

```java
Flagger.shutdown(5000)
```

### publish

```java
public static void publish(Entity entity) 
```

Explicitly notify Airdeploy about an Entity

```java
Attributes attributes = new Attributes().put("age", 40);
Flagger.publish(Entity.builder().id("37").attributes(attributes).build());
```

### track

```java
public static void track(Event event) {
```

Event tracking API.
Entity is an optional parameter if it was set before.

```java
Attributes eventProperties = new Attributes()
    .put("plan", "gold")
    .put("referrer", "www.google.com")
    .put("shirt_size", "medium");
Event event = Event.builder().name("Purchase Completed")
    .eventProperties(eventProperties)
    .entity(Entity.builder()
                    .id("37")
                    .build())
    .build();
Flagger.track(event);
```

### setEntity

```java
public static void setEntity(Entity entity)
```

`setEntity` stores an entity in Flagger, which allows omission of entity in other API methods. 

```java
Entity entity = Entity.builder().id(whiteListedEntityUserId).build();

Flagger.setEntity(entity);
boolean enabled = Flagger.flagIsEnabled(flagCodenameWhitelisted, null);

Assert.assertTrue(enabled);
//clean up
Flagger.setEntity(null);
```

>If you don't provide __any__ entity to Flagger:
>- flag functions always resolve with the default variation
>- `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

## Flag Functions
### flagIsEnabled

```java
public static boolean flagIsEnabled(String codename, Entity entity) 
```

Determines if flag is enabled for entity.

```java
boolean enabled = Flagger.flagIsEnabled("test", Entity.builder().id("id").build());
```


Group example:

```java
Entity company = Entity.builder()
                .id("random")
                .group(GroupEntity.builder().id("companyId").type("Company").build()).build();
boolean enabledCompany = Flagger.flagIsEnabled("some-flag", company);
```


### flagIsSampled

```java
public static boolean flagIsSampled(String codename, Entity entity) 
```

Determines if entity is within the targeted subpopulations

```java
boolean isSampled = Flagger.flagIsSampled("show_wallet", generateEntity());
```

Group example:

```java
Entity company = Entity.builder()
                .id("random")
                .group(GroupEntity.builder().id("companyId").type("Company").build()).build();
boolean isSampled = Flagger.flagIsSampled("show_wallet", company)
```

### flagGetVariation

```java
public static String flagGetVariation(String codename, Entity entity)
```

Returns the variation assigned to the entity in a multivariate flag

```java
String variation = Flagger.flagGetVariation("show_wallet", generateEntity());
```

Group example:

```java
Entity company = Entity.builder()
                .id("random")
                .group(GroupEntity.builder().id("companyId").type("Company").build()).build();

String variation = Flagger.flagGetVariation("show_wallet", company);
```

### flagGetPayload

```java
public static Map<String, Object> flagGetPayload(String codename, Entity entity)
```

Returns the payload associated with the treatment assigned to the entity

```java
Map<String, Object> payload = Flagger.flagGetPayload("show_new_dashboard", someEntity);
```

Group example:

```java
Entity company = Entity.builder()
                .id("random")
                .group(GroupEntity.builder().id("companyId").type("Company").build()).build();

Map<String, Object> payload = Flagger.flagGetPayload("show_new_dashboard", company);
```