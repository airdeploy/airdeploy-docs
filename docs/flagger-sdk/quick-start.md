---
id: quick-start
title: Quick Start
sidebar_label: Quick Start
---

Integrating Flagger into your app or website can begin as soon as you create an Airdeploy account, requiring only four steps:

1. Obtain an API key so `Flagger` can authenticate your API requests
2. Install a client library
3. Test the installation
4. Call `Flagger.shutdown`

## Obtain an API Key

To initialize `Flagger` with an API key. Otherwise, `Flagger` will return
[Default Variation](default-variation.md) for any [Flag Function](flag-functions.md) call.

Flagger is initialized with an API key that identifies which environment to set up. Before initialization is complete, Flagger returns the [default variation](default-variation.md) for any method call.

Every environment in Airdeploy has a pair of keys, which are available through the dashboard.

## Install a client library

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```commandline
npm install --save flagger
```

<!--React-->

```commandline
npm install --save flagger-react
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
go get github.com/airdeploy/flagger-go/v3
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

<!--Swift (Cocoapod)-->

Add Flagger to your Podfile:

```
  pod 'Flagger'
```

and then install pod:

`pod install`

[More info on how to manage your pods](https://cocoapods.org/)

<!--END_DOCUSAURUS_CODE_TABS-->

## Test the installation

Now, let's test that our installation is working.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

<br>First, import the Flagger library in your application code:

```javascript
import Flagger from 'flagger'
```

To initialize, Flagger requires only 1 network request. This request is made by the `init()` method. Due to nature of Javascript,
`init()` is a promise, so we must wait for it to resolve before calling any other `Flagger` methods.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

> Note: for a refresher on promises, you can reference this [article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

```javascript
await Flagger.init({apiKey: <API_KEY>, logLevel: 'debug'})
```

To ensure that Flagger is successfully initialized, call any flag function. For this test, we will use `isEnabled`.

```javascript
console.log(Flagger.isEnabled('test-flag', {id: '1'}))
```

The result will be `false` printed in a console.

<!--React-->

<br>First, import the Flagger library.

The Flagger React library contains components that encapsulate the logic of checking whether or not to render a component, and will trigger updates (re-rendering) in real-time if changes are made to the flag (i.e. toggling the flag on / off in the dashboard).

The `FlagProvider` component can be inserted into the application so that Flagger is initialized when the application starts.

```javascript
import {FlagProvider, FlagSwitch, Flag, Variation, withFlag, useVariation, useFlag} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
    // insert rest of app
  </FlagProvider>
)
```

The `Flag` component renders its children based on whether the `case` prop matches the flag variation. The entity is inherited from FlagProvider if provided. If one was not provided to the `FlagProvider` component, or you would like to override the entity, one can be provided to the Flag component as the `entity` prop.

```javascript
<FlagProvider apiKey="<API_KEY>" entity={user}>
  <Flag case="on" flag="color-theme">
    <NewColorComponent />
  </Flag>
</FlagProvider>
```

<!--Ruby-->

<br>First, import the Flagger library in your application code:

```ruby
require 'flagger'
```

To initialize, Flagger requires only 1 network request. This request is made by the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```ruby
api_key = '<API_KEY>'
Flagger::init(api_key, log_level: "debug)
```

To ensure that Flagger is successfully initialized, call any flag function. For this test, we will use `is_enabled`.

```ruby

p Flagger::is_enabled('test-flag', Flagger::Entity::new('1'))
```

The result will be `false` printed in a console.

<!--Python-->

<br>First, import the Flagger library in your application code:

```python
import flagger
```

To initialize, Flagger requires only 1 network request. This request is made by the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```python
flagger.init(api_key="<API_KEY>", log_lvl="debug")
```

To ensure that Flagger is successfully initialized, call any flag function. For this test, we will use `is_enabled`.

```python
print(flagger.is_enabled("test-flag", {"id": "1"}))
```

The result will be `false` printed in a console.

<!--Go-->

<br>First, import the Flagger library in your application code:

```go
import "github.com/airdeploy/flagger-go/v3"
```

To initialize, Flagger requires only 1 network request. This request is made by the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```go

// Flagger uses logrus as a logger
// By default Flagger will output all warn and error
logrus.SetLevel(logrus.DebugLevel) // set to debug to see all messages

ctx := context.Background()
err := flagger.Init(ctx, &flagger.InitArgs{APIKey: "<API_KEY>"})
```

To ensure that Flagger is successfully initialized, call any flag function. For this test, we will use `isEnabled`.

```go
log.Println(flagger.isEnabled(ctx, "test-flag", &core.Entity{ID: "1"}))
```

The result will be `false` printed in a console.

<!--Java-->

<br>First, import the Flagger library in your application code:

```java
import io.airdeploy.flagger.*;
```

To initialize, Flagger requires only 1 network request. This request is made by the `init()` method.

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

```java
String apiKey = "<API_KEY>";
FlaggerInitConfig flaggerInitConfig = FlaggerInitConfig.builder()
                          .apiKey(apiKey)
                          .logLevel(LogLevel.DEBUG)
                          .build();
Flagger.init(flaggerInitConfig);
```

To ensure that Flagger is successfully initialized, call any flag function. For this test, we will use `isEnabled`.

```java
Entity entity = Entity.builder().id("1").build();
System.out.println(Flagger.isEnabled("test-flag", entity));
```

The result will be `false` printed in a console.

<!--Swift-->

Initialize Flagger, as soon as your app starts, for example In AppDelegate.swift:

```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // todo: Override point for customization after application launch.
        Flagger.initialize(apiKey: "<API_KEY>", logLevel: LogLevel.debug)
        return true
    }

```

then, use Flagger in your components:

```swift
let entity = Entity(id: "1")
let isEnabled = Flagger.isEnabled(codename: "test-flag", entity: entity)
```

<!--END_DOCUSAURUS_CODE_TABS-->

## Shutdown Flagger

The `shutdown` method should be called **once** before your application exits to flush any data Flagger has not sent.

> Note: If you use Flagger in a browser-based application (like a single-page application), this is not necessary.

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

<!--Swift-->

In `AppDelegate.swift`:

```swift
    func applicationWillTerminate(_ application: UIApplication) {
        _ = Flagger.shutdown(timeoutMillis: 1000)
    }
```

<!--END_DOCUSAURUS_CODE_TABS-->

## Summary

At this point, Flagger has been installed, initialized, tested, and shut down. The result of the test is a `false` enabled result for a flag printed to console. This will also populate a new flag in the Airdeploy dashboard with the codename `test-flag`.
