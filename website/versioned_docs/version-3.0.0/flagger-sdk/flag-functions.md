---
id: version-3.0.0-flag-functions
title: Flag Functions
sidebar_label: Flag Functions
original_id: flag-functions
---
## Overview
Once you've installed a Flagger library, you'll have access to all the flags in a particular environment. 
Flag functions are the main way developers interact with Airdeploy to fork behavior in code, relying on 
Airdeploy to resolve all the different feature flag rules.

## Flag functions
There are 4 main functions used for controlling the flow:
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
Flagger.flagIsEnabled(codename: String, entity?: Entity): Boolean

Flagger.getVariation(codename: String, entity?: Entity): String

Flagger.isSampled(codename: String, entity?: Entity): Boolean

Flagger.getPayload(codename: String, entity?: Entity): Object

```
<!--Ruby-->
```ruby
Flagger.flag_is_enabled(codename, *entity)

Flagger.get_variation(codename, *entity)

Flagger.is_sampled(codename, *entity)

Flagger.get_payload(codename, *entity)
```
<!--Python-->
```python
flagger.flag_is_enabled(codename, entity=None)

flagger.get_variation(codename, entity=None)

flagger.is_sampled(codename, entity=None)

flagger.get_payload(codename, entity=None)
```
<!--Go-->
```go
func FlagIsEnabled(codename string, entity *core.Entity) bool
 
func FlagGetVariation(codename string, entity *core.Entity) bool

func FlagIsSampled(codename string, entity *core.Entity) bool
 
func FlagGetPayload(codename string, entity *core.Entity) bool 
```
<!--Java-->
```java
public static boolean flagIsEnabled(String codename, Entity entity) 

public static String flagGetVariation(String codename, Entity entity) 

public static boolean flagIsSampled(String codename, Entity entity) 

public static Map<String, Object> flagGetPayload(String codename, Entity entity) 
```
<!--END_DOCUSAURUS_CODE_TABS-->

> `Flagger` has `setEntity` method which set default `entity` for the whole SDK. 
>That's why `entity` attribute is optional in each Flag Function.  

## Main Flag Functions
These two functions you will probably use the most.

### flagIsEnabled
Check whether a flag is enabled for an entity
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const enabled = Flagger.flagIsEnabled('color-theme', {id: "1"})

if (enabled) {
  // show new color button
} else {
  // show old color button
}
```
<!--Ruby-->
```ruby
enabled = Flagger.flag_is_enabled('color-theme', Entity::new("1"))

if enabled
  # show new color button
else 
  # show old color button
end
```
<!--Python-->
```python
enabled = flagger.flag_is_enabled('color-theme', {"id": "1"})

if enabled:
    # show new color button
else:
    # show old color button
```
<!--Go-->
```go
import 	"github.com/jeronimo13/flagger-sdks/flagger/core"

enable := flagger.FlagIsEnabled("color-theme", &core.Entity{ID: "1"})
	
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
boolean enabled = Flagger.flagIsEnabled("color-theme", entity);

if(enabled){
    // show new color button
} else {
    // show old color button
}
```
<!--END_DOCUSAURUS_CODE_TABS-->


### flagGetVariation
Returns the variation that the entity will receive (after resolving all Flagging Rules). 
This is a more general flag function that is useful for multivariate flags.
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const variation = Flagger.flagGetVariation('color-theme', {id: "1"})

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
variation = Flagger.flag_get_variation('color-theme', Entity::new("1"))

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
variation = Flagger.flag_get_variation('color-theme', {id: "1"})

if variation == 'halloween': 
  print('show orange and black button')
elif variation == 'christmas':
  print('show green and red button')
elif variation == 'coca-cola':
  print('show red and black button')

```
<!--Go-->
```go
import 	"github.com/jeronimo13/flagger-sdks/flagger/core"

variation := flagger.FlagGetVariation("color-theme", &core.Entity{ID: "1"})
	
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
String variation = Flagger.flagGetVariation("color-theme", entity);

if(variation == "halloween"){
  // show orange and black button
} else if(variation == "christmas"){
  // show green and red button
} else if (variation == "coca-cola"){
  // show red and black button
}
```
<!--END_DOCUSAURUS_CODE_TABS-->

> flagGetVariation returns 'off' as the default variation (see [Default Variation](./default-variation.md))

## Helpful Flag Functions
These two functions are less often-used, but are helpful in some cases.

### flagIsSampled
Returns whether or not an entity is within one of the targeted populations (see [Flags](../airdeploy/flags.md)). 
However, the entity may or may not be "sampled". A sampled entity may someday receive this feature, but this function only determines whether entity is sampled.
<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const isSampled = Flagger.flagIsSampled('color-theme', {id: "1"})
```
<!--Ruby-->
```ruby
sampled = Flagger.flag_is_sampled('color-theme', Entity::new("1"))
```
<!--Python-->
```python
sampled = flagger.flag_is_sampled('color-theme', {"id": "1"})
```
<!--Go-->
```go
import 	"github.com/jeronimo13/flagger-sdks/flagger/core"

enable := flagger.flagIsSampled("color-theme", &core.Entity{ID: "1"})
```
<!--Java-->
```java
import io.airdeploy.flagger.entity.Entity;

Entity entity = Entity.builder().id("1").build();
boolean isSampled = Flagger.flagIsSampled("color-theme", entity);
```
<!--END_DOCUSAURUS_CODE_TABS-->

### flagGetPayload
Returns a JSON payload that can be added to any treatment via the Airdeploy Dashboard.

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```typescript
const payload = Flagger.flagGetPayload('color-theme', {id: "1"})
// => {"button-color": "blue"}
```
<!--Ruby-->
```ruby
payload = Flagger.flag_get_payload('color-theme', Entity::new("1"))
# => {"button-color": "blue"}
```
<!--Python-->
```python
payload = Flagger.flag_get_payload('color-theme', {id: "1"})
# => {"button-color": "blue"}
```
<!--Go-->
```go
import 	"github.com/jeronimo13/flagger-sdks/flagger/core"

payload := flagger.FlagGetPayload("color-theme", &core.Entity{ID: "1"})
// => {"button-color": "blue"}
```
<!--Java-->
```java
import io.airdeploy.flagger.entity.Entity;

Entity entity = Entity.builder().id("1").build();
Map<String, Object> payload = Flagger.flagGetPayload("color-theme", entity);
// => {"button-color": "blue"}
```
<!--END_DOCUSAURUS_CODE_TABS-->