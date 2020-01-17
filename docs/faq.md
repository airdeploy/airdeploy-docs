---
id: faq
title: FAQ
sidebar_label: FAQ
---

- Why do all flag functions return `false`/`off`/`{}`(empty payload)?

The common problem we see that `Flagger` is not be initialized properly. The correct way to do it:
```typescript
//first call init Promise and wait for it to resolve
await Flagger.init({"apiKey":"ashvsidvlds"})

Flagger.flagIsEnabled("my-flag", {id:"2342"})
// assuming `my-flag` is on and id "2342" is in the whitelist
// => true
```


  