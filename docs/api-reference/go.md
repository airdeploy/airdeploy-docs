---
id: go
title: Golang API Reference
sidebar_label: Golang
---

## Flagger

### Init

```go
func (flagger *Flagger) Init(args *InitArgs) error
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `Init` must be called only once, at the start of your application.
> Your program **must** wait for `Init` to finish before using any other `Flagger` methods

```go
import (
	"context"
	"fmt"
	"github.com/airdeploy/flagger-go/v3"
	"github.com/sirupsen/logrus"
)

func main() {
	err := flagger.Init(&flagger.InitArgs{
		APIKey: "<API-KEY>", // could be omitted if FLAGGER_API_KEY env variable is set
		LogLevel: "DEBUG", // could be omitted, ERROR by default
	})

	if err != nil {
		panic(fmt.Sprintf("Error during Flagger initialization: %s", err))
	}

    // rest of the app
}
```

| name            | Environment variable      | Default                                     | Description                                                                                             |
| --------------- | ------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| APIKey          | FLAGGER_API_KEY           | None                                        | API key to an environment                                                                               |
| SourceURL       | FLAGGER_SOURCE_URL        | https://flags.airdeploy.io/v3/config/       | URL to get `FlaggerConfiguration`                                                                       |
| BackupSourceURL | FLAGGER_BACKUP_SOURCE_URL | https://backup-api.airshiphq.com/v3/config/ | backup URL to get `FlaggerConfiguration`                                                                |
| SSEURL          | FLAGGER_SSE_URL           | https://sse.airdeploy.io/v3/sse/            | URL for real-time updates of `FlaggerConfiguration` via sse                                             |
| IngestionURL    | FLAGGER_INGESTION_URL     | https://ingestion.airdeploy.io/v3/ingest/   | URL for ingestion                                                                                       |
| LogLevel        | FLAGGER_LOG_LEVEL         | ERROR                                       | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

- If `APIKey` argument is not provided and `FLAGGER_API_KEY` environment variable is not set then `Init` returns an error "Bad init agrs" and print an error in the console: "empty APIKey"
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

> Note: you **must** call `Shutdown` only once before the end of the application runtime.

```go
flagger.Shutdown(5 * time.Second)
```

### Publish

```go
func (flagger *Flagger) Publish(entity *core.Entity)
```

Explicitly notifies Airdeploy about an Entity

```go
flagger.Publish(&core.Entity{ID: "54"})
```

### Track

```go
func (flagger *Flagger) Track(event *core.Event)
```

Event tracking API.
Entity is an optional parameter if it was set before.

```go
flagger.Track(&core.Event{
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
enabled := flagger.IsEnabled("new-signup-flow", nil)
nonEmptyVariation := flagger.GetVariation("new-signup-flow", nil)
assert.True(t, enabled)
assert.Equal(t, "enabled", nonEmptyVariation)

flagger.SetEntity(nil)
```

> If you don't provide **any** entity to Flagger:
>
> - flag functions always resolve with the default variation
> - `Track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

### IsEnabled

```go
func (flagger *Flagger) IsEnabled(codename string, entity *core.Entity) bool
```

Determines if flag is enabled for entity.

```go
flagger.IsEnabled("test", &core.Entity{ID: "1"})
```

### IsSampled

```go
func (flagger *Flagger) IsSampled(codename string, entity *core.Entity) bool
```

Determines if entity is within the targeted subpopulations

```go
entity := &core.Entity{
    ID:         "kfjvv3",
    Attributes: core.Attributes{"admin": true},
}

sampled := flagger.IsSampled("premium-support", entity)
```

### GetVariation

```go
func (flagger *Flagger) GetVariation(codename string, entity *core.Entity) string
```

Returns the variation assigned to the entity in a multivariate flag

```go

entity := &core.Entity{
    ID:         "kfjvv3",
    Attributes: core.Attributes{"admin": true},
}

variation := flagger.GetVariation("premium-support", entity)
```

### GetPayload

```go
func (flagger *Flagger) GetPayload(codename string, entity *core.Entity) core.Payload
```

Returns the payload associated with the treatment assigned to the entity

```go
payload := flagger.GetPayload("enterprise-dashboard", &core.Entity{ID: "31404847", Type: "Company"})
```
