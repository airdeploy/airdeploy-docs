---
id: ingestion
title: Ingestion
sidebar_label: Ingestion
---

Flagger collects every "decision" that was made (for example, whether `isEnabled` true or false for a given entity), and then sends it to populate the Airdeploy dashboard. This data is used to populate known entities, set up filters, and preview how flag assignments will be made.

Common rules:

- Flagger saves ingestion data locally and sends them to Airdeploy periodically.

Browser specific rules:

- Flagger ingests every 250 ms to insure no data is lost due to window closing.

Server specific rules:

- Flagger sends empty ingestion upon init to notify Airdeploy about brand new sdk installation.

- First 10 flag function usages are always ingested.

- Shutdown Flagger gracefully at the end of the application's runtime to send cached ingestion:

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
let isTimeOut = Flagger.shutdown(timeoutMillis: 1000)
```

<!--END_DOCUSAURUS_CODE_TABS-->
