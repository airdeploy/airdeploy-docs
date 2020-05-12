---
id: go
title: Golang API Reference
sidebar_label: Golang
---

## Flagger
### Init

```go
func (flagger *Flagger) Init(ctx context.Context, args *InitArgs) error
```

`Init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initialize Ingester

> Note: `Init` must be called only once, at the start of your application. 
>Your program __must__ wait for `Init` to finish before using any other `Flagger` methods


```go
import (
	"context"
	"fmt"
	"github.com/jeronimo13/flagger-sdks/flagger"
	"github.com/sirupsen/logrus"
)

func main() {
	logrus.SetLevel(logrus.DebugLevel) // Flagger uses logrus as a logger

	ctx := context.Background()
	err := flagger.Init(ctx, &flagger.InitArgs{
		APIKey:          "x2ftC7QtG7arQW9l", // the only required field
		SourceURL:       "https://flagger.notairdeploy.io",
		BackupSourceURL: "https://backupflagger.notairdeploy.io",
		SSEURL:          "https://sse.notairdeploy.io",
		IngestionURL:    "https://ingestion.notairdeploy.io",
	})
	
	if err != nil {
		panic(fmt.Sprintf("Error during Flagger initialization: %s", err))
	}

    // rest of the app
}
```

| name            | type   | Required | Default                           | Description                                                                                             |
| --------------- | ------ | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| APIKey          | string | true     | None                              | API key to an environment                                                                               |
| SourceURL       | string | false    | https://api.airdeploy.io/configurations/        | URL to get `FlaggerConfiguration`                                                                         |
| BackupSourceURL | string | false    | https://backup-api.airdeploy.io/configurations/ | backup URL to get `FlaggerConfiguration`                                                                  |
| SSEURL          | string | false    | https://sse.airdeploy.io/sse/v3/?envKey=        | URL for real-time updates of `FlaggerConfiguration` via sse                                                                       |
| IngestionURL    | string | false    | https://ingestion.airdeploy.io/collector?envKey=   | URL for ingestion                                                                                       |

- If `APIKey` is not provided `Init` returns an error "Bad init agrs" and print an error in the console: "empty APIKey"
- If not provided default arguments values are used and printed to Debug
- If second(third …) call of `Init` happens:
    - If the arguments are the same, `Init` method does nothing
    - If arguments differ, `Flagger` prints warnings and recreates(closes and creates new) resources(SSE connection, 
    Ingester, gets new `FlaggerConfiguration`).
    - > Note: you must call init only once
- If initial `FlaggerConfiguration` is not fetched from source/backup, Flagger prints a warning
- If `Flagger` fails to get `FlaggerConfiguration` then all Flags Functions return [Default Variation](../flagger-sdk/default-variation.md)
- If Flagger fails to establish SSE connection, it retries every 30 seconds until succeeded
- If you call any Flag Function BEFORE `init` is finished then you'll get [Default Variation](../flagger-sdk/default-variation.md)  
- To change default log level use `logrus.SetLevel`. Flagger uses 3 log levels: `debug`, `warn` and `error` 

### Shutdown

```go
func (flagger *Flagger) Shutdown(timeout time.Duration) bool 
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`Shutdown` waits until current ingestion request is finished, but no longer than a `timeout`.

returns `true` if closed by timeout 

> Note: you __must__ call `Shutdown` only once before the end of the application runtime. 

```go
flagger.Shutdown(5 * time.Second)
```

### Publish

```go
func (flagger *Flagger) Publish(ctx context.Context, entity *core.Entity)
```

Explicitly notify Airdeploy about an Entity

```go
flagger.Publish(ctx, &core.Entity{ID: "54"})
```

### Track

```go
func (flagger *Flagger) Track(ctx context.Context, event *core.Event)
```

Event tracking API.
Entity is an optional parameter if it was set before.

```go
flagger.Track(ctx, &core.Event{
    Name: "test",
    EventProperties: core.Attributes{
        "plan":       "Bronze",
        "referrer":   "www.Google.com",
        "shirt_size": "medium",
    },
    Entity: &core.Entity{ID: "1"},
})
```

### SetEntity

```go
func (flagger *Flagger) SetEntity(entity *core.Entity) {
```

`SetEntity` stores an entity in Flagger, which allows omission of entity in other API methods. 

```go
flagger.SetEntity(&core.Entity{ID: "90843823"})
enabled := flagger.FlagIsEnabled("new-signup-flow", nil)
nonEmptyVariation := flagger.FlagGetVariation("new-signup-flow", nil)
assert.True(t, enabled)
assert.Equal(t, "enabled", nonEmptyVariation)

flagger.SetEntity(nil)
```

>If you don't provide __any__ entity to Flagger:
>- flag functions always resolve with the default variation
>- `Track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

## Flag Functions
### FlagIsEnabled

```go
func (flagger *Flagger) FlagIsEnabled(codename string, entity *core.Entity) bool 
```

Determines if flag is enabled for entity.

```go
flagger.FlagIsEnabled("test", &core.Entity{ID: "1"})
```

### FlagIsSampled

```go
func (flagger *Flagger) FlagIsSampled(codename string, entity *core.Entity) bool 
```

Determines if entity is within the targeted subpopulations

```go
entity := &core.Entity{
    ID:         "kfjvv3",
    Attributes: core.Attributes{"admin": true},
}

sampled := flagger.FlagIsSampled("premium-support", entity)
```

### FlagGetVariation

```go
func (flagger *Flagger) FlagGetVariation(codename string, entity *core.Entity) string 
```

Returns the variation assigned to the entity in a multivariate flag

```go

entity := &core.Entity{
    ID:         "kfjvv3",
    Attributes: core.Attributes{"admin": true},
}

variation := flagger.FlagGetVariation("premium-support", entity)
```


### FlagGetPayload

```go
func (flagger *Flagger) FlagGetPayload(codename string, entity *core.Entity) core.Payload 
```

Returns the payload associated with the treatment assigned to the entity

```go
payload := flagger.FlagGetPayload("enterprise-dashboard", &core.Entity{ID: "31404847", Type: "Company"})
```
