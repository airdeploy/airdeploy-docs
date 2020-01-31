---
id: quick-start
title: Quick Start
sidebar_label: Quick Start
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
API keys are available in the Airship Dashboard.

## Install a client library

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```commandline
npm install --save flagger
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
go get github.com/airshiphq/flagger-go
```
<!--Java-->
Maven
```xml
<dependency>
  <groupId>com.airshiphq</groupId>
  <artifactId>flagger</artifactId>
  <version>3.0.0</version>
</dependency>
```
Gradle
```commandline
compile "com.airshiphq:flagger:3.0.0"
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Test the installation

Now, let's test that our installation is correct

>Note: We include randomly generated API key in our code examples

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
<!--Go-->
<br>First, import `flagger` to your application code:
```go
import "github.com/airshiphq/flagger-sdks/flagger"
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
<!--Java-->
<br>First, import `flagger` to your application code:
```java
import com.airshiphq.flagger.*;
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
IdEntity entity = IdEntity.builder().id("1").build();
System.out.println(Flagger.flagIsEnabled("test", entity));
```
<!--END_DOCUSAURUS_CODE_TABS-->
 
The result would be `false` printed in console. It happens because `Airship` doesn't know about flag with codename 
"test", in turn `Flagger` doesn't have it in `FlaggerConfiguration` making `flagIsEnabled` to return `false`. 

`Flagger` automatically detects any new flags. See [Flag Detection](flag-detection.md)

## Shutdown Flagger

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

> Note: If your application doesn't call `shutdown` before the end of the runtime then __there is a risk that all 
>accumulated data is lost__.

> Note: __do not__ call `shutdown` more than __once per runtime__.

## Summary

At this point we initialized `Flagger`, called Flag Function and shut down `Flagger`.
The result of it is `false` printed in the console and a new flag with a codename `"test"` in Airship Dashboard 