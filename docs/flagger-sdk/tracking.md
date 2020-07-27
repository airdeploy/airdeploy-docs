---
id: tracking
title: Tracking API
sidebar_label: Tracking API
---

Flagger has a simple event tracking API:

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```javascript
Flagger.track(eventName: String,
                  eventPayload: JSON,
                  entity?: Entity  //optional
                 )
```

<!--Ruby-->

```ruby
Flagger.track(name, event_properties, *entity)
```

<!--Python-->

```python
Flagger.track(event_name, event_props, entity=None)
```

<!--Go-->

```go
Track(ctx context.Context, event *core.Event)
```

<!--Java-->

```java
public static void track(Event event)
```

<!--END_DOCUSAURUS_CODE_TABS-->

Entity is an optional parameter if it was set before via `Flagger.setEntity` method

## Examples

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->

```js
Flagger.track(
  'Purchase Completed',
  {
    plan: 'Gold',
    referrer: 'https://www.google.com',
    shirt_size: 'medium',
  },
  {id: '543'}
)

// If `entity` has been set before with Flagger.setEntity method:
Flagger.track('Purchase Completed', {
  plan: 'Gold',
  referrer: 'https://www.google.com',
  shirt_size: 'medium',
})
```

<!--Ruby-->

```ruby
Flagger.track('Purchase Completed',
  {:plan => "Gold",
   :referrer => "https://www.google.com",
   :shirt_size => "medium"},
  FlaggerClasses::Entity::new("42", Hash::new)
)

# If `entity` has been set before with Flagger.set_entity method:
Flagger.track('Purchase Completed',
  {:plan => "Gold",
   :referrer => "https://www.google.com",
   :shirt_size => "medium"}
)
```

<!--Python-->

```python
flagger.track("Purchase Completed", {"plan": "Gold", "referrer": "https://www.google.com", "shirt_size": "medium"}, {"id": "543"})

# If `entity` has been set before with Flagger.set_entity method:
flagger.track("Purchase Completed", {"plan": "Gold", "referrer": "https://www.google.com", "shirt_size": "medium"})
```

<!--Go-->

```go
flagger.Track(ctx, &core.Event{
			Name: "Purchase Completed",
			EventProperties: core.Attributes{
				"plan":       "Bronze",
				"referrer":   "www.google.com",
				"shirt_size": "medium",
			},
			Entity: &core.Entity{ID: "42"},
		})

// If `entity` has been set before with flagger.SetEntity method:
flagger.Track(ctx, &core.Event{
			Name: "Purchase Completed",
			EventProperties: core.Attributes{
				"plan":       "Bronze",
				"referrer":   "www.google.com",
				"shirt_size": "medium",
			},
		})
```

<!--Java-->

```java
Attributes eventProperties = new Attributes()
    .put("plan", "Bronze")
    .put("referrer", "www.google.com")
    .put("shirt_size", "medium",);
Entity entity = Entity.builder().id("42")
        .build();
Event event = new Event("Purchase Completed", eventProperties, entity);
Flagger.track(event);
```

<!--END_DOCUSAURUS_CODE_TABS-->

> You must provide to Flagger an entity, either via 3rd parameter in **Flagger.track** method or via **Flagger.setEntity**, which will set
> entity globally for all of the Flagger methods
