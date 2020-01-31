---
id: flag-functions
title: Flag Functions
sidebar_label: Flag Functions
---
## Overview
Once you've installed a Flagger library, you'll have access to all the flags in a particular environment. 
Flag functions are the main way developers interact with Airship to fork behavior in code, relying on 
Airship to resolve all the different feature flag rules.

## Flag functions
There are 4 main functions used for controlling the flow:
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
Flagger.flagIsEnabled(codename: String, entity?: Entity): Boolean

Flagger.isSampled(codename: String, entity?: Entity): Boolean

Flagger.getVariation(codename: String, entity?: Entity): String

Flagger.getPayload(codename: String, entity?: Entity): Object

```
<!--Ruby-->
```ruby
Flagger.flag_is_enabled(codename, *entity)

Flagger.is_sampled(codename, *entity)

Flagger.get_variation(codename, *entity)

Flagger.get_payload(codename, *entity)
```
<!--Python-->
```python
flagger.flag_is_enabled(codename, entity=None)

flagger.is_sampled(codename, entity=None)

flagger.get_variation(codename, entity=None)

flagger.get_payload(codename, entity=None)
```
<!--Go-->
```go
```
<!--Java-->
```java
```
<!--END_DOCUSAURUS_CODE_TABS-->

> `Flagger` has `setEntity` method which defines Entity for the whole SDK so Entity could be omitted in Flag Function. That's why entity 
>attribute is optional here 

## Main Flag Functions
These two functions are the ones you need to know.

### flagIsEnabled
Get whether a flag is enabled for an entity
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const enabled = Flagger.flagIsEnabled('bitcoin-pay', {id: "1"})
// => true

if (enabled) {
  // show bitcoin pay button
} else {
  // show normal credit card payment button
}
```
<!--Ruby-->
```ruby
```
<!--Python-->
```python
```
<!--Go-->
```go
```
<!--Java-->
```java
```
<!--END_DOCUSAURUS_CODE_TABS-->


### flagGetVariation
Returns the variation that the entity will receive (after resolving all Flagging Rules). 
This is a more general flag function that is useful for multivariate flags.
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const variation = Flagger.flagGetVariation('color-theme', {id: "1"})
// => halloween

if (variation === 'halloween') {
  // show orange and black button
} else if (variation === 'christmas') {
  // show green and red button
} else if (variation === 'coca-cola') {
  // show red and black button
}
```
<!--Ruby-->
```ruby
```
<!--Python-->
```python
```
<!--Go-->
```go
```
<!--Java-->
```java
```
<!--END_DOCUSAURUS_CODE_TABS-->

>    flagGetVariation returns 'off' as the default variation


## Helpful Flag Functions
These two functions are less often-used, but are helpful in some cases.

### flagIsSampled
Returns whether or not an entity is within one of the targeted populations (see [Airship Flags](../flagger/flags.md)). 
However, the entity may or may not be "sampled". A sampled entity may someday receive this feature, but this function only determines whether entity is sampled.
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const isSampled = Flagger.flagIsSampled('bitcoin-pay', {id: "1"})
// => true
```
<!--Ruby-->
```ruby
```
<!--Python-->
```python
```
<!--Go-->
```go
```
<!--Java-->
```java
```
<!--END_DOCUSAURUS_CODE_TABS-->

### flagGetPayload
Returns a JSON payload that can be added to any treatment via the Airship Dashboard.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const payload = Flagger.flagGetPayload('bitcoin-pay', {id: "1"})
// => {"bitcoin-price": "$9001"}
```
<!--Ruby-->
```ruby
```
<!--Python-->
```python
```
<!--Go-->
```go
```
<!--Java-->
```java
```
<!--END_DOCUSAURUS_CODE_TABS-->