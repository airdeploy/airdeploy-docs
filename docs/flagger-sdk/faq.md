---
id: faq
title: FAQ
sidebar_label: FAQ
---

## Why do all flag functions always return `false`/`off`/`{}`(empty payload)?

- Check that you are using correct API Key. API Keys differ for different environments (test, dev, prod etc.)

- Flagger may not be initialized properly. Check to make sure Flagger is initialized before using the library.

  In Javascript, the `init()` method is a promise; make sure it's resolved before calling any other functions.

  ```typescript
  //first call init Promise and wait for it to resolve
  await Flagger.init({apiKey: '<API-KEY>'})

  Flagger.isEnabled('my-flag', {id: '2342'})
  // assuming `my-flag` is on and id "2342" is in the whitelist
  // => true
  ```

- Entity may not sampled. Subpopulation sampling percentage is set to 0% by default (which prevents any traffic from going through the flag). Check that it is set to the appropriate value.

- Check that the flag codename is correct. Make sure you are using codename and not the name of the flag. For example, a flag named "New Dashboard" may have a codename of `new-dashboard`.

## Should I call Flagger.init method every time I want to use Flag Function?

No, you should call `Flagger.init` only once, at the start of your application. Doing so more than once negates the benefit of how Flagger is architected, since more network calls are made that are necessary.
