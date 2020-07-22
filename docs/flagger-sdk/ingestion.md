---
id: ingestion
title: Ingestion
sidebar_label: Ingestion
---

Flagger collects every "decision" that was made (for example, whether `flagIsEnabled` true or false for a given entity), and then sends it to populate the Airdeploy dashboard. This data is used to populate known entities, set up filters, and preview how flag assignments will be made.

This process is fully automatic and requires only one thing from the developer - to gracefully shutdown Flagger.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```javascript
await Flagger.shutdown() // shutdown is a Promise
```

<!--Ruby-->

```ruby
Flagger.shutdown(3000)
```

<!--Python-->

```python
Flagger.shutdown(3000)
```

<!--Go-->

```go
Flagger.shutdown(3000)
```

<!--Java-->

```java
Flagger.shutdown(3000)
```

<!--Swift-->

```swift
// iOS
func applicationWillTerminate(_ application: UIApplication) {
    _ = Flagger.shutdown(timeoutMillis: 1000)
}

// anywhere else:
_ = Flagger.shutdown(timeoutMillis: 1000)
```

<!--END_DOCUSAURUS_CODE_TABS-->
