---
id: flag-functions
title: Flag Functions
sidebar_label: Flag Functions
---

## Overview

Once you've installed a Flagger library, you'll have access to all the flags in a particular environment. Flag functions are the main way developers interact with Flagger to fork behavior in code.

## Flag Functions

There are 4 main functions used for controlling traffic flow:

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```javascript
Flagger.isEnabled(codename: String, entity?: Entity): Boolean

Flagger.getVariation(codename: String, entity?: Entity): String

Flagger.isSampled(codename: String, entity?: Entity): Boolean

Flagger.getPayload(codename: String, entity?: Entity): Object

```

<!--Ruby-->

```ruby
Flagger.is_enabled(codename, *entity)

Flagger.get_variation(codename, *entity)

Flagger.is_sampled(codename, *entity)

Flagger.get_payload(codename, *entity)
```

<!--Python-->

```python
flagger.is_enabled(codename, entity=None)

flagger.get_variation(codename, entity=None)

flagger.is_sampled(codename, entity=None)

flagger.get_payload(codename, entity=None)
```

<!--Go-->

```go
func IsEnabled(codename string, entity *core.Entity) bool

func GetVariation(codename string, entity *core.Entity) string

func IsSampled(codename string, entity *core.Entity) bool

func GetPayload(codename string, entity *core.Entity) core.Payload
```

<!--Java-->

```java
public static boolean isEnabled(String codename, Entity entity)

public static String getVariation(String codename, Entity entity)

public static boolean isSampled(String codename, Entity entity)

public static Map<String, Object> getPayload(String codename, Entity entity)
```

<!--END_DOCUSAURUS_CODE_TABS-->

> Flagger has a `setEntity` method which sets a default `entity`. If an entity is not provided to the flag function, the default entity, if it has been set, is used to evaluate the flag.

## Most Commonly Used Functions

These two functions you will probably use the most.

### isEnabled

Check whether a flag is enabled for an entity

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```typescript
const enabled = Flagger.isEnabled('color-theme', {id: '1'})

if (enabled) {
  // show new color button
} else {
  // show old color button
}
```

<!--Ruby-->

```ruby
enabled = Flagger.is_enabled('color-theme', FlaggerClasses::Entity::new("1"))

if enabled
  # show new color button
else
  # show old color button
end
```

<!--Python-->

```python
enabled = flagger.is_enabled('color-theme', {"id": "1"})

if enabled:
    # show new color button
else:
    # show old color button
```

<!--Go-->

```go
import 	"github.com/airdeploy/flagger-go/core"

enable := flagger.IsEnabled("color-theme", &core.Entity{ID: "1"})

if enable {
// show new color button
} else {
// show old color button
}
```

<!--Java-->

```java
import io.airdeploy.flagger.entity.Entity;


Entity entity = Entity.builder().id("1").build();
boolean enabled = Flagger.isEnabled("color-theme", entity);

if(enabled){
    // show new color button
} else {
    // show old color button
}
```

<!--END_DOCUSAURUS_CODE_TABS-->

### getVariation

Returns the variation that the entity will receive (after resolving all Flagging Rules).
This is a more general flag function that is useful for multivariate flags.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```typescript
const variation = Flagger.getVariation('color-theme', {id: '1'})

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
variation = Flagger.get_variation('color-theme', FlaggerClasses::Entity::new("1"))

case variation
when 'halloween'
  # show orange and black button
when 'christmas'
  # show green and red button
when 'coca-cola'
  # show red and black button
end
```

<!--Python-->

```python
variation = Flagger.get_variation('color-theme', {id: "1"})

if variation == 'halloween':
  print('show orange and black button')
elif variation == 'christmas':
  print('show green and red button')
elif variation == 'coca-cola':
  print('show red and black button')

```

<!--Go-->

```go
import 	"github.com/airdeploy/flagger-go/core"

variation := flagger.GetVariation("color-theme", &core.Entity{ID: "1"})

if variation == "halloween" {
  // show orange and black button
} else if variation == "christmas" {
  // show green and red button
} else if variation == "coca-cola" {
  // show red and black button
}
```

<!--Java-->

```java
import io.airdeploy.flagger.entity.Entity;

Entity entity = Entity.builder().id("1").build();
String variation = Flagger.getVariation("color-theme", entity);

if(variation == "halloween"){
  // show orange and black button
} else if(variation == "christmas"){
  // show green and red button
} else if (variation == "coca-cola"){
  // show red and black button
}
```

<!--END_DOCUSAURUS_CODE_TABS-->

> getVariation returns 'off' as the default variation (see [Default Variation](./default-variation.md))

## Other Helpful Functions

These two functions are less often-used, but are helpful in some cases.

### isSampled

Returns whether or not an entity is within one of the targeted populations.
However, the entity may or may not be "sampled". A sampled entity may someday receive this feature, but this function only determines whether entity is sampled.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```typescript
const isSampled = Flagger.isSampled('color-theme', {id: '1'})
```

<!--Ruby-->

```ruby
sampled = Flagger.is_sampled('color-theme', FlaggerClasses::Entity::new("1"))
```

<!--Python-->

```python
sampled = flagger.is_sampled('color-theme', {"id": "1"})
```

<!--Go-->

```go
import 	"github.com/airdeploy/flagger-go/core"

enable := flagger.IsSampled("color-theme", &core.Entity{ID: "1"})
```

<!--Java-->

```java
import io.airdeploy.flagger.entity.Entity;

Entity entity = Entity.builder().id("1").build();
boolean isSampled = Flagger.isSampled("color-theme", entity);
```

<!--END_DOCUSAURUS_CODE_TABS-->

### getPayload

Returns a JSON payload that can be added to any treatment via the Airdeploy Dashboard.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```typescript
const payload = Flagger.getPayload('color-theme', {id: '1'})
// => {"button-color": "blue"}
```

<!--Ruby-->

```ruby
payload = Flagger.get_payload('color-theme', FlaggerClasses::Entity::new("1"))
# => {"button-color": "blue"}
```

<!--Python-->

```python
payload = Flagger.get_payload('color-theme', {id: "1"})
# => {"button-color": "blue"}
```

<!--Go-->

```go
import 	"github.com/airdeploy/flagger-go/core"

payload := flagger.GetPayload("color-theme", &core.Entity{ID: "1"})
// => {"button-color": "blue"}
```

<!--Java-->

```java
import io.airdeploy.flagger.entity.Entity;

Entity entity = Entity.builder().id("1").build();
Map<String, Object> payload = Flagger.getPayload("color-theme", entity);
// => {"button-color": "blue"}
```

<!--END_DOCUSAURUS_CODE_TABS-->
