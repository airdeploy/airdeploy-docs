---
id: version-3.0.x-swift
title: Swift API Reference
sidebar_label: Swift
original_id: swift
---

## Flagger
### initialize

```swift
public static func initialize(apiKey: String, logLevel: LogLevel = LogLevel.error) -> Void  
public static func initialize(apiKey: String, sourceURL: String, backupSourceURL: String, sseURL: String, ingestionURL: String, logLevel: LogLevel = LogLevel.error) -> Void
```

Use this function once at the start of the application

 ```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    Flagger.initialize(apiKey: "ufu4gf04fh")
    
    return true
}
```



| name            | type   | Required | Default                                         | Description                                                                                             |
| --------------- | ------ | -------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| apiKey          | string | true     | None                                            | API key to an environment                                                                               |
| sourceUrl       | string | false    | https://api.airdeploy.io/configurations/        | URL to get `FlaggerConfiguration`                                                                       |
| backupSourceURL | string | false    | https://backup-api.airdeploy.io/configurations/ | backup URL to get `FlaggerConfiguration`                                                                |
| sseUrl          | string | false    | https://sse.airdeploy.io/sse/v3/?envKey=        | URL for real-time updates of `FlaggerConfiguration` via sse                                             |
| ingestionUrl    | string | false    | https://ingestion.airdeploy.io/collector?envKey=| URL for ingestion                                                                                       |
| logLevel        | string | false    | ERROR                                           | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

- If second(third …) call of `initialize` happens:
    - If the arguments are the same, `initialize` method does nothing
    - If arguments differ, `Flagger` prints warnings and recreates(closes and creates new) resources(SSE connection, 
    Ingester, gets new `FlaggerConfiguration`).
    - > Note: you must call initialize only once
- If initial `FlaggerConfiguration` is not fetched from source/backup, Flagger prints a warning
- If `Flagger` fails to get `FlaggerConfiguration` then all Flags Functions return [Default Variation](../flagger-sdk/default-variation.md)
- If Flagger fails to establish SSE connection, it retries every 30 seconds until succeeded
- If you call any Flag Function BEFORE `initialize` is finished then you'll get [Default Variation](../flagger-sdk/default-variation.md)  


### shutdown

```swift
public static func shutdown(timeoutMillis: Int) -> Bool
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`shutdown` waits until current ingestion request is finished, but no longer than a timeoutMillis.

returns `true` if closed by timeout 

> Note: you __must__ call shutdown only once before the end of the application runtime. 

Probably the best place to call shutdown is at `applicationWillTerminate` method:
```swift
func applicationWillTerminate(_ application: UIApplication) {
    _ = Flagger.shutdown(timeoutMillis: 1000)
}
```

### publish
```swift
public static func publish(_ entity: Entity) -> Void
```

Explicitly notify Airdeploy about an Entity

```swift
Flagger.publish(Entity("343223"))
```

### track

```swift
public static func track(_ event: Event) -> Void {
```

Event tracking API.
Entity is an optional parameter if it was set before.

```swift
let entity = Entity(id: "57145770", type: "User", group: Group(id: "321", attributes:Attributes().put(key: "isAdmin", value: true)))
let event = Event(name: "test", attributes: Attributes().put(key: "isAdmin", value: true), entity: entity)
Flagger.track(event)

Flagger.track(Event(name: "test", attributes: Attributes().put(key: "isAdmin", value: true)))
```

### setEntity
```swift
public static func setEntity(_ entity: Entity?) -> Void
```

`setEntity` stores an entity in Flagger, which allows omission of entity in other API methods. 


```swift
let entity = Entity(id: "57145770")
Flagger.setEntity(nil) // resets any entity that flagger could have
XCTAssertFalse(Flagger.flagIsEnabled(codename: "group-messaging"))

Flagger.setEntity(entity)
XCTAssert(Flagger.flagIsEnabled(codename: "group-messaging")) // entity is provided by setEntity

//clean up
Flagger.setEntity(nil)
XCTAssertFalse(Flagger.flagIsEnabled(codename: "group-messaging"))
```

>If you don't provide __any__ entity to Flagger:
>- flag functions always resolve with the default variation
>- `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

## Flag Functions
### flagIsEnabled

```swift
public static func flagIsEnabled(codename: String, entity: Entity) -> Bool
public static func flagIsEnabled(codename: String) -> Bool
```

Determines if flag is enabled for entity.

```swift
XCTAssert(Flagger.flagIsEnabled(codename: "group-messaging", entity: Entity(id: "57145770")))
XCTAssertFalse(Flagger.flagIsEnabled(codename: "group-messaging", entity: Entity(id: "57145771")))
```

Group example:
```swift
XCTAssert(Flagger.flagIsEnabled(codename: "group-messaging", entity: Entity(id: "randomid", group: Group(id: "4576815", type: "Company"))))
```

### flagIsSampled

```swift
public static func flagIsSampled(codename: String, entity: Entity) -> Bool
public static func flagIsSampled(codename: String) -> Bool
```

Determines if entity is within the targeted subpopulations

```swift
let attributes: Attributes = Attributes().put(key:"createdAt", value:"2014-09-20T00:00:00Z")
XCTAssertTrue(Flagger.flagIsSampled(codename: "company-profiles", entity: Entity(id: "9139fdsds5", attributes: attributes)))
        
// group example
XCTAssertTrue(Flagger.flagIsSampled(codename: "org-chart", entity: Entity(id: "41", type: "User", group: Group(id:"543", type:"Company"))))
```

### flagGetVariation

```swift
public static func flagGetVariation(codename: String, entity: Entity) -> String
public static func flagGetVariation(codename: String) -> String
```

Returns the variation assigned to the entity in a multivariate flag

```swift
XCTAssertEqual(Flagger.flagGetVariation(codename: "group-messaging", entity: Entity("57145770")), "enabled")
```

### flagGetPayload

```swift
public static func flagGetPayload(codename: String, entity: Entity) -> [String:Any]
public static func flagGetPayload(codename: String) -> [String:Any] 
```

Returns the payload associated with the treatment assigned to the entity

```swift
let payload =  Flagger.flagGetPayload(codename: "faq-redesign", entity: Entity(id: "92784783"))
if let showButtonsPayload = payload["show-buttons"]{
  
    if let showButtons = showButtonsPayload as? Bool{
        XCTAssert(showButtons)
    } else {
        XCTFail("Must be Bool")
    }
} else {
    XCTFail("Must return payload")
}
```