---
id: overview
title: Overview
sidebar_label: Overview
---

`Flagger SDK`(or `Flagger` for short) is the open-source implementation of the feature flagging(feature gating, feature toggles) concept. 

## Features
- Blazing fast, requires only one backend call
- Highly customizable
- Sampling(canary release) and multivariate A/B testing with custom filters
- Auto-updatable configuration(via SSE)
- Records usage analytics as well as custom events
- white- and blacklisting
- 2 level entity support (for example Manager-Employee, Client-Company)


## Design Principles
### Stateful
`Flagger` is stateful SDK, it relies on `FlaggerConfiguration` to work. This config is essential for `Flagger`, so 
`Flagger` needs to be initialized before used.

### Initialize before use 
`Flagger` makes an http call to Airship server to get `FlaggerConfiguration`. 
This fact imposes the restrictions on your application, since now 
your application has to rely on how fast `Flagger` makes this http call. No worries, Airship uses CDN to make 
initialization as fast as possible.  

The trade off is that any other `Flagger` methods doesn't require any http call, making them extremely fast

 >Note: You must initialized `Flagger` __only once per runtime__. See [Test Flagger installation](quick-start.md#test-the-installation) 

### Terminate at the end of the runtime
`Flagger` accumulates usage data, we call it _exposure_, for instance:
```json
{
  "codename": "button",
  "variation": "green",
  "entity": {"id":  "1"},
  "methodCalled": "isEnabled"
}
```

This data allows Airship to show A/B results and lots of other important things. `Flagger` groups this data up before 
sending to Airship to decrease network usage and Airship server load. If your application stops without properly 
shutting down `Flagger` all the accumulated data will be lost. See [Shutdown Flagger](quick-start.md#shutdown-flagger)   

 >Note: You must call `Flagger.shutdown` __once before the end of the runtime__ 


### All methods are static
It makes it really easy to use `Flagger` from any point of you application.

### Auto updatable configuration
`Flagger` uses Server Side Events to make sure `FlaggerConfiguration` stays up to date. During the `init` method `Flagger` 
establishes and then maintains a connection with Airship enabling it to push new config.

That is why your application does not need to restart to get the new `FlaggerConfiguration`. Your app will get new 
FlaggerConfiguration as soon as you make changes in the Dashboard.

### SDK implementation details
Typescript and Golang version of Flagger is developed from scratch, the rest is the wrapper around the native build of 
Golang library. Native code is built via xgo for Linux, Mac, Win for x86 and x32 architectures. 