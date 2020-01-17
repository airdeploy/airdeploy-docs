---
id: ingestion
title: Ingestion
sidebar_label: Ingestion
---

Flagger collects every "decision" that was made(for example whether flagIsEnabled true or false for a 
given entity), groups this decisions up in "ingestion requests" and then sends it to the Airship.

In the nutshell ingestion is the process of collecting the results of flag functions and sending it to Airship. 
This process is fully automatic and requires only one thing from the developer - to gracefully shutdown Flagger SDK at 
the end of the application runtime:

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```javascript
await Flagger.shutdown()
```
<!--Ruby-->
```ruby
Flagger.shutdown()
```
<!--Python-->
```python
Flagger.shutdown()
```
<!--Go-->
```go
Flagger.shutdown()
```
<!--Java-->
```java
Flagger.shutdown()
```
<!--END_DOCUSAURUS_CODE_TABS-->