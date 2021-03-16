---
id: ingestion
title: Ingestion
sidebar_label: Ingestion
---

Flagger collects every "decision" that was made (for example, whether `isEnabled` true or false for a given entity), and then sends it to populate the Airdeploy dashboard. This data is used to populate known entities, set up filters, and preview how flag assignments will be made.

Flagger caches ingestions and sends them to Airdeploy automatically.

Shutdown Flagger gracefully at the end of the application's runtime to send cached ingestion:

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```javascript
await Flagger.shutdown() // shutdown is a Promise
```

<!--Ruby-->

```ruby
Flagger::shutdown(3000)
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
let isTimeOut = Flagger.shutdown(timeoutMillis: 1000)
```

<!--PHP-->

```
Flagger::shutdown(3000)
```

<!--END_DOCUSAURUS_CODE_TABS-->
