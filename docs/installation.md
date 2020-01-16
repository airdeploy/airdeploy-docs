---
id: installation
title: Installation
sidebar_label: Installation
---


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

Your API keys are available in the Dashboard.

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
