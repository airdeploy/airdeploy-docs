---
id: version-3.0.0-quick-start
title: Quick Start
sidebar_label: Quick Start
original_id: quick-start
---

Integrating `Flagger` into your app or website can begin as soon as you create a `Flagger` account, 
requiring only four steps:

1. Obtain your API key so `Flagger` can authenticate your API requests
2. Install a client library
3. Test the installation
4. Call `Flagger.shutdown`

## Obtain your API key

You must initialize `Flagger` with your account's API key. Otherwise, `Flagger` will return 
[Default Variation](default-variation.md) for any [Flag Function](flag-functions.md) call.

Every account is provided with two pairs of keys: one for the testing and one for the production environment. These 
API keys are available in the Airdeploy Dashboard.

## Install a client library

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```commandline
npm install --save flagger
```
<!--React-->
```commandline
npm install --save react-flagger
```
<!--Ruby-->
```commandline
gem install flagger
```
<!--Python-->
```commandline
pip install flagger
```
<!--Go-->
```commandline
go get github.com/airdeploy/flagger-go
```
<!--Java-->
Maven
```xml
<dependency>
  <groupId>io.airdeploy</groupId>
  <artifactId>flagger</artifactId>
  <version>3.0.0</version>
</dependency>
```
Gradle
```commandline
compile "io.airdeploy:flagger:3.0.0"
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Test the installation

Now, let's test that our installation is correct

>We included randomly generated API key in our code examples

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
<br>First, import `Flagger` to your application code:
```javascript
import Flagger from 'flagger'
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method. Due to javascript nature,
`init()` is a Promise, so we must wait for it to resolve before calling any other `Flagger` methods. 

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

>Note: If you are not familiar with the concept of the Promise we recommend to read this 
>[article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 
```javascript
await Flagger.init({apiKey: 'x2ftC7QtG7arQW9l'})
```

To insure Flagger is successfully initialized we may call any flag function, let's call `flagIsEnabled` for example:
```javascript
console.log(Flagger.flagIsEnabled('test', {id: '1'}))
```

The result will be `false` printed in console.

<!--React-->
<br>Initialize Airdeploy to connect to your environment, which fetches data for all feature flags and experiments in that environment.
```javascript
import {FlagProvider, FlagSwitch, Flag, withFlag} from 'react-flagger'

const App = () => (
  <FlagProvider envKey="YOUR_ENV_KEY" entity={user}>
    {// insert rest of app}
  </FlagProvider>
)
```
React Flagger contains components that are useful for feature gating; 
they encapsulate the logic of checking whether or not to render a component, 
and updates (re-renders) the component in real-time if you make changes on the Airdeploy dashboard.

The Flag component renders its children based on whether the case prop matches the flag variation. 
The entity is inherited from FlagProvider if provided; 
if not, make sure to provide a user / entity object to the flag component.

```javascript
<FlagProvider envKey="YOUR_ENV_KEY" entity={user}>
  <Flag case="on" flag="color-theme">
    <NewColorComponent />
  </Flag>
</FlagProvider>
```

<!--Ruby-->
<br>First, import `flagger` to your application code:
```ruby
require 'flagger'
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```ruby
api_key = 'x2ftC7QtG7arQW9l'
args = InitArguments::new(api_key)
Flagger.init(args)
```

To insure Flagger is successfully initialized we may call any flag function, let's call `flagIsEnabled` for example:
```ruby
p Flagger.flagIsEnabled('test', {id: '1'})
```

The result will be `false` printed in console.

<!--Python-->
<br>First, import `flagger` to your application code:
```python
import flagger
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```python
flagger.init("x2ftC7QtG7arQW9l")
```

To insure Flagger is successfully initialized we may call any flag function, let's call `flagIsEnabled` for example:
```python
print(flagger.flag_is_sampled("test", {"id": "1"}))
```

The result will be `false` printed in console.

<!--Go-->
<br>First, import `flagger` to your application code:
```go
import "github.com/airdeploy/flagger-go"
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```go
ctx := context.Background()
err := flagger.Init(ctx, &flagger.InitArgs{APIKey: "x2ftC7QtG7arQW9l"})
```

To insure Flagger is successfully initialized we may call any flag function, let's call `flagIsEnabled` for example:
```go
log.Println(flagger.FlagIsEnabled(ctx, "test", &core.Entity{ID: "1"}))
```

The result will be `false` printed in console.

<!--Java-->
<br>First, import `flagger` to your application code:
```java
import io.airdeploy.flagger.*;
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```java
String apiKey = "x2ftC7QtG7arQW9l";
FlaggerInitConfig flaggerInitConfig = FlaggerInitConfig.builder()
                          .apiKey(apiKey)
                          .build();
Flagger.init(flaggerInitConfig);
```

To insure Flagger is successfully initialized we may call any flag function, let's call `flagIsEnabled` for example:
```java
Entity entity = Entity.builder().id("1").build();
System.out.println(Flagger.flagIsEnabled("test", entity));
```
The result will be `false` printed in console.

<!--END_DOCUSAURUS_CODE_TABS-->
 
You can use a flag that `Airdeploy` doesn't know about yet.
If `Flagger` doesn't have it in `FlaggerConfiguration`, `flagIsEnabled` returns `false`.
 
`Flagger` automatically detects any new flags. See [Flag Detection](flag-detection.md)

## Shutdown Flagger

>Note: If you use `Flagger` in a browser, this part is irrelevant. `Flagger` sends ingestion data immediately to avoid data loss

You must call `shutdown` method __once__ before the end of your application's runtime to make sure that 
`Flagger SDK` sends all the accumulated ingestion data:

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
await Flagger.shutdown()
```
<!--Ruby-->
```ruby
Flagger.shutdown(5000) # shutdown takes a timeout as an argument
```
<!--Python-->
```python
Flagger.shutdown(5000) # shutdown takes a timeout as an argument
```
<!--Go-->
```go
Flagger.shutdown(5000) // shutdown takes a timeout as an argument
```
<!--Java-->
```java
Flagger.shutdown(5000) // shutdown takes a timeout as an argument
```
<!--END_DOCUSAURUS_CODE_TABS-->

> Note: If your application doesn't call `shutdown` before the end of the runtime then __there is a risk that 
>accumulated data is lost__.

> Note: __do not__ call `shutdown` more than __once per runtime__.

## Summary

At this point we initialized `Flagger`, called Flag Function and shut down `Flagger`.
The result of it is `false` printed in the console and a new flag with a codename `"test"` in Airdeploy Dashboard 