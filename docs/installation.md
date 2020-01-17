---
id: installation
title: Installation
sidebar_label: Installation
---

Integrating Flagger into your app or website can begin as soon as you create a Flagger account, 
requiring only three steps:

1. Obtain your API key so Flagger can authenticate your API requests
2. Install a client library
3. Call flagIsEnabled function

### Obtain your API key

If you don't initialize Flagger with your account's API key Flagger will return 
[default variation](#default-variation) for any [flag request](#flags).

Every account is provided with two pairs of keys: one for the testing and one for the production environment. 

Your API keys are available in the Dashboard.

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

## Call flagIsEnabled function

Now, let's test that our installation is correct and try to get some value of the Flagger

>We include randomly generated API key in our code examples

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
<br>First, import `Flagger` to your application code:
```javascript
import Flagger from 'flagger'
```

`Flagger` requires only 1 network request. This request is implemented in the `init()` method. Due to javascript nature,
`init()` is a Promise, so we must wait for it to resolve before calling any other `Flagger` methods. 

A natural way of calling `init()` is to do it only once per runtime, at the start of the application.

>If you are not familiar with the concept of the Promise we recommend to read this 
>[article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 
```javascript
await Flagger.init({apiKey: 'x2ftC7QtG7arQW9l'})
```

After Flagger is successfully initialized we can call our `flagIsEnabled` function:
```javascript
console.log(Flagger.flagIsEnabled('test', {id: '1'}))
```
<!--Ruby-->
```ruby
require 'flagger'

api_key = 'x2ftC7QtG7arQW9l'
args = InitArguments::new(api_key)
Flagger.init(args)

p Flagger.flag_is_enabled('test', {id: '1'})
```
<!--Python-->
```python
import flagger
import time

flagger.init("x2ftC7QtG7arQW9l")

print(flagger.flag_is_sampled("test", {"id": "1"}))
time.sleep(1) # wait for the flagger to ingest 
```
<!--Go-->
```go
func main() {
	ctx := context.Background()
	logrus.SetLevel(logrus.DebugLevel)

	_ = flagger.Init(ctx, &flagger.InitArgs{APIKey: "x2ftC7QtG7arQW9l"})

	flagger.FlagIsEnabled(ctx, "test", &core.Entity{ID: "1"})
	time.Sleep(1 * time.Second) // wait for the flagger to ingest 
}
```
<!--Java-->
```java
class Main {
    public static void main(String[] args){
          String apiKey = "x2ftC7QtG7arQW9l";
          FlaggerInitConfig flaggerInitConfig = FlaggerInitConfig.builder()
                          .apiKey(apiKey)
                          .build();
          Flagger.init(flaggerInitConfig);
          IdEntity entity = IdEntity.builder().id("1").build();
          System.out.println(Flagger.flagIsEnabled("test", entity));
    }
}
```
<!--END_DOCUSAURUS_CODE_TABS-->
 
The result would be `false` printed in console. It happens because `Flagger` doesn't know about flag with codename "test".
>If your program is not finished right after calling `flagIsEnabled` then eventually Flagger will _ingest_ the flag 
"test" and you will see it in the Dashboard. 


Call this before the end of your application/script:
```typescript
...

await Flagger.shutdown()
```
to make sure that `Flagger SDK` send all the ingestion data.
