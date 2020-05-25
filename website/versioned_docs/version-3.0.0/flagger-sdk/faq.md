---
id: version-3.0.0-faq
title: FAQ
sidebar_label: FAQ
original_id: faq
---

## Why do all flag functions return `false`/`off`/`{}`(empty payload)?

The common problem we see that `Flagger` is not be initialized properly. The correct way to do it:
```typescript
//first call init Promise and wait for it to resolve
await Flagger.init({"apiKey":"ashvsidvlds"})

Flagger.flagIsEnabled("my-flag", {id:"2342"})
// assuming `my-flag` is on and id "2342" is in the whitelist
// => true
```


## Should I call Flagger.init method every time I want to use Flag Function?
 
 No, you should call `Flagger.init` only once, at the start of your application.