---
id: faq
title: FAQ
sidebar_label: FAQ
---

### Why do all flag functions always return `false`/`off`/`{}`(empty payload)?

Flagger may not be initialized properly. Check to make sure Flagger is initialized before using the library.

In Javascript, the `init()` method is a promise; make sure it's resolved before calling any other functions.

```typescript
//first call init Promise and wait for it to resolve
await Flagger.init({apiKey: 'ashvsidvlds'})

Flagger.flagIsEnabled('my-flag', {id: '2342'})
// assuming `my-flag` is on and id "2342" is in the whitelist
// => true
```

### Should I call Flagger.init method every time I want to use Flag Function?

No, you should call `Flagger.init` only once, at the start of your application. Doing so more than once negates the benefit of how Flagger is architected, since more network calls are made that are necessary.
