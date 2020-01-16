---
id: overview
title: Overview
sidebar_label: Overview
---

Flagger is the open-source implementation of the feature flagging(feature gating, feature toggles) concept. 
It works both in the browser and on the server. 

#### Client libraries:
- Nodejs
- React
- Java 
- Python
- Ruby
- Go

## Features
- Blazing fast does not require a backend call to decide what flag to show
- Highly customizable
- Sampling(canary release) and multivariate A/B testing with custom filters
- Auto-updatable configuration(via SSE)
- Records usage analytics as well as custom events
- white- and blacklisting
- 2 level entity support (Manager-Employee, Client-Company)

High level speaking Flagger contains two different parts: a client library and a server.
There are two options for the server: self-hosted community version and Airship-hosted Professional version.

## Design Principles
Description: I'm trying to have each docs section have a "Design Principles" section. Let's see if it fits.

## Installation
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```shell script
npm install --save flagger`
```
<!--Ruby-->
```shell script
gem install flagger
```
<!--Python-->
```shell script
pip install flagger
```
<!--Go-->
```shell script
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
```shell script
compile "com.airshiphq:flagger:3.0.0"
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Initialization / Configuration

Integrating Flagger into your app or website can begin as soon as you create a Flagger account, 
requiring only three steps:

1. Obtain your API key so Flagger can authenticate your API requests
2. Install a client library
3. Make a test Flag request


### Obtain your API key

If you don't initialize Flagger with your account's API key Flagger will return 
[default variation](#default-variation) for any [flag request](#flags).

Every account is provided with two pairs of keys: one for the testing and one for the production environment. 

Your API keys are available in the Dashboard(LINK HERE).

## Install a client library

We provide official libraries for different programming languages and mobile platforms. 
Check out [installation section](#installation)

## Make a test Flag request

```text
    📝 Note: We include randomly generated API key in our code examples
```

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```javascript
import Flagger from 'flagger'

await Flagger.init({apiKey: 'x2ftC7QtG7arQW9l'})
Flagger.flagIsEnabled('test', {id: '1'})

const p = new Promise(r=> {
  setTimeout(()=> {
      r()
  }, 1000)
})
await p
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
 
The result would be `false` printed in console and a flag named test ingested in the Dashboard

## Flags
Description: Show how to use Flagger to set up different types of flags. 
Simple on/off flags, multivariate experiments, etc. 
How to use different functions / methods.
.getVariation()
.isEnabled()
.isSampled()
.getPayload()

##### Default variation:
```json
{
	"isEnabled": false,
	"isSampled": false,
	"variation": "off",
	"payload": {}
}
```

## Tracking Events
Description: How to use Flagger to track events / analytics

## Flag Detection
Description: How to use Flagger to create new flags.

## Order of Precedence
Description: How to use Flagger to track events/analytics

## API Reference
Description: Full API reference
