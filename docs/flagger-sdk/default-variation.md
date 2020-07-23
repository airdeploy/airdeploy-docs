---
id: default-variation
title: Default Variation
sidebar_label: Default Variation
---

The default variation is the default return value for the [Flag Functions](flag-functions.md).

```json
{
  "isEnabled": false,
  "isSampled": false,
  "variation": "off",
  "payload": {}
}
```

The default variation can be returned for a number of reasons, including:

- The flag's kill switch is on
- Entity/group is whitelisted to the default variation
- Entity/group is not part of the sampled subpopulation
