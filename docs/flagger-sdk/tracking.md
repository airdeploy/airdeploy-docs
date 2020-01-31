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
<!--END_DOCUSAURUS_CODE_TABS-->

Entity is an optional parameter if it was set before via `Flagger.setEntity` method

<!--DOCUSAURUS_CODE_TABS-->
<!--Javascript-->
```js
Flagger.track('Purchase Completed', {
            "plan": "Gold",
      "referrer": "www.Google.com",
      "shirt_size": "medium"
    }, {id: "543"})
    
// If `entity` has been set before with Flagger.setEntity method:
Flagger.track('Purchase Completed', {
        "plan": "Gold",
  "referrer": "www.Google.com",
  "shirt_size": "medium"
})
```
<!--END_DOCUSAURUS_CODE_TABS-->



>You must provide to Flagger an entity, either via 3rd parameter in __Flagger.track__ method or via __Flagger.setEntity__, which will set 
>entity globally for all of the Flagger methods 
