---
id: ruby
title: Ruby API Reference
sidebar_label: Ruby
---

## Flagger

### init

```ruby
def self.init(init_args)
```

`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `init` must be called only once, at the start of your application.
> Your program **must** wait for `init` to finish before using any other `Flagger` methods

```ruby
api_key = '<API-KEY>' # could be omitted if FLAGGER_API_KEY env variable is set
log_level = "debug" # could be omitted, error by default
Flagger::init(api_key, log_level: log_level)
```

| name          | Environment variable      | Default                                     | Description                                                                                             |
| ------------- | ------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| api_key       | FLAGGER_API_KEY           | None                                        | API key to an environment                                                                               |
| source_url    | FLAGGER_SOURCE_URL        | https://flags.airdeploy.io/v3/config/       | URL to get `FlaggerConfiguration`                                                                       |
| backup_url    | FLAGGER_BACKUP_SOURCE_URL | https://backup-api.airshiphq.com/v3/config/ | backup URL to get `FlaggerConfiguration`                                                                |
| sse_url       | FLAGGER_SSE_URL           | https://sse.airdeploy.io/v3/sse/            | URL for real-time updates of `FlaggerConfiguration` via sse                                             |
| ingestion_url | FLAGGER_INGESTION_URL     | https://ingestion.airdeploy.io/v3/ingest/   | URL for ingestion                                                                                       |
| log_lvl       | FLAGGER_LOG_LEVEL         | ERROR                                       | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

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

```ruby
def self.shutdown(timeout)
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`shutdown` waits until current ingestion request is finished, but no longer than a `timeout`.

returns `true` if closed by timeout

> Note: you **must** call shutdown only once before the end of the application runtime.

```ruby
Flagger::shutdown(1000)
```

### publish

```ruby
  def self.publish(entity)
```

Explicitly notify Airdeploy about an Entity

```ruby
Flagger::publish(Flagger::Entity::new("42"))

# or

Flagger::publish({id: "42"})
```

### track

```ruby
  def self.track(name, event_properties, entity)
```

Event tracking API.
Entity is an optional parameter if it was set before.

```ruby
Flagger::track('test', {:age => 40}, Flagger::Entity::new("42"))
```

### set_entity

```ruby
  def self.set_entity(entity)
```

`set_entity` stores an entity in Flagger, which allows omission of entity in other API methods.

```ruby
Flagger::set_entity(Flagger::Entity::new("42"))

# here we are omitting, because Flagger has already stored "entity"
Flagger::is_enabled('show_wallet')
```

> If you don't provide **any** entity to Flagger:
>
> - flag functions always resolve with the default variation
> - `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

### is_enabled

```ruby
  def self.is_enabled(codename, *entity)
```

Determines if flag is enabled for entity.

```ruby
is_enabled= Flagger::is_enabled('show_wallet', Flagger::Entity::new("42"))
```

Group example:

```ruby
is_enabled= Flagger::is_enabled('show_wallet', {id:"42", group: {id:"4242", type: 'company'}})
```

### is_sampled

```ruby
  def self.is_sampled(codename, *entity)
```

Determines if entity is within the targeted subpopulations

```ruby
is_sampled= Flagger::is_sampled('show_wallet', Flagger::Entity::new("42"))
```

Group example:

```ruby
is_sampled= Flagger::is_sampled('show_wallet', {id:"42", group: {id:"4242", type: 'company'}})
```

### get_variation

```ruby
  def self.get_variation(codename, *entity)
```

Returns the variation assigned to the entity in a multivariate flag

```ruby
variation = Flagger::get_variation("show_wallet", Flagger::Entity::new("42"))
```

Group example:

```ruby
variation = Flagger::get_variation('show_wallet', {id:"42", group: {id:"4242", type: 'company'}})
```

### get_payload

```ruby
  def self.get_payload(codename, *entity)
```

Returns the payload associated with the treatment assigned to the entity

```ruby
payload = Flagger::get_payload('show_wallet', Flagger::Entity::new("42"))
```

Group example:

```ruby
payload = Flagger::get_payload('show_wallet', {id:"42", group: {id:"4242", type: 'company'}})
```
