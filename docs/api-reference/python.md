---
id: python
title: Python API Reference
sidebar_label: Python
---

## Flagger
### init

```python
def init(api_key, source_url=None, backup_url=None, sse_url=None, ingestion_url=None,
         log_lvl="error")
```



`init` method gets `FlaggerConfiguration`, establishes and maintains SSE connections and initializes Ingester

> Note: `init` must be called only once, at the start of your application. 
>Your program __must__ wait for `init` to finish before using any other `Flagger` methods

```python
flagger.init(api_key="x2ftC7QtG7arQW9l",
             source_url="https://flagger.notairdeploy.io",
             backup_url="https://backupflagger.notairdeploy.io",
             sse_url="https://sse.notairdeploy.io",
             ingestion_url="https://ingestion.notairdeploy.io",
             log_lvl="warn")
```

| name            | type   | Required | Default                           | Description                                                                                             |
| --------------- | ------ | -------- | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| api_key         | string | true     | None                              | API key to an environment                                                                               |
| source_url      | string | false    | https://api.airdeploy.io/configurations/        | URL to get `FlaggerConfiguration`                                                                         |
| backup_url      | string | false    | https://backup-api.airdeploy.io/configurations/ | backup URL to get `FlaggerConfiguration`                                                                  |
| sse_url         | string | false    | https://sse.airdeploy.io/sse/v3/?envKey=        | URL for real-time updates of `FlaggerConfiguration` via sse                                                                       |
| ingestion_url   | string | false    | https://ingestion.airdeploy.io/collector?envKey=   | URL for ingestion                                                                                       |
| log_lvl         | string | false    | ERROR                             | set up log level: ERROR, WARN, DEBUG. Debug is the most verbose level and includes all Network requests |

- If `api_key` is not provided `init` throws an RuntimeError: "bad init arguments" and print error in console: "empty APIKey"  
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

```python
def shutdown(timeout)
```

`shutdown` ingests data(if any), stops ingester and closes SSE connection.
`shutdown` waits until current ingestion request is finished, but no longer than a `timeout`.

returns `true` if closed by timeout 

> Note: you __must__ call shutdown only once before the end of the application runtime. 

```python
flagger.shutdown(5000)
```

### publish

```python
def publish(entity)
```

Explicitly notify Airdeploy about an Entity

```python
flagger.publish({"id": "1"})
```

### track

```python
def track(event_name, event_props, entity=None)
```

Event tracking API.
Entity is an optional parameter if it was set before.

```python
flagger.track("flag-codename", {"test": True}, {"id": "1"})
```

### set_entity

```python
def set_entity(entity)
```

`set_entity` stores an entity in Flagger, which allows omission of entity in other API methods. 

```python
flagger.set_entity(entity={"id": "1"})

# here we are omitting, because Flagger has already stored "entity"
ok = flagger.flag_is_enabled(codename="some-flag-codename")
self.assertTrue(ok)
```

>If you don't provide __any__ entity to Flagger:
>- flag functions always resolve with the default variation
>- `track` method doesn't record an event

Rule of thumb: make sure you always provide an entity to the Flagger

## Flag Functions
### flag_is_enabled

```python
def flag_is_enabled(codename, entity=None)
```

Determines if flag is enabled for entity.

```python
is_enabled = flagger.flag_is_enabled(codename="show_wallet", entity={"id": "3201"})
```

Group example:

```python
is_enabled = flagger.flag_is_enabled(codename="show_wallet", 
            entity={
             "id": "3306",
             "group": {
                 "id": "432423",
                 "type": "Company",
             }})
```


### flag_is_sampled

```python
def flag_is_sampled(codename, entity=None)
```

Determines if entity is within the targeted subpopulations

```python
is_sampled = flagger.flag_is_sampled(codename="show_wallet", entity={"id": "3201"})
```

Group example:

```python
is_sampled = flagger.flag_is_sampled(codename="show_wallet", 
            entity={
             "id": "3306",
             "group": {
                 "id": "432423",
                 "type": "Company",
             }})
```

### flag_get_variation

```python
def flag_get_variation(codename, entity=None)
```

Returns the variation assigned to the entity in a multivariate flag

```python
variation = flagger.flag_get_variation(
                             codename="show_wallet",
                             entity={"id": "3201"})
```

Group example:

```python
variation = flagger.flag_get_variation(
                             codename="show_wallet",
                             entity={
                                  "id": "3306",
                                  "group": {
                                      "id": "432423",
                                      "type": "Company",
                                  }})
```

### flag_get_payload

```python
def flag_get_payload(codename, entity=None)
```

Returns the payload associated with the treatment assigned to the entity

```python
payload = flagger.flag_get_payload(
                             codename="show_wallet",
                             entity={"id": "3201"})
```

Group example:

```python
payload = flagger.flag_get_payload(
                             codename="show_wallet",
                             entity={
                                  "id": "3306",
                                  "group": {
                                      "id": "432423",
                                      "type": "Company",
                                  }})
```