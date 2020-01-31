---
id: default-variation
title: Default Variation
sidebar_label: Default Variation
---

## Overview
The default variation is the default return value for the [Flag Functions](flag-functions.md).
```json
{
	"isEnabled": false,
	"isSampled": false,
	"variation": "off",
	"payload": {}
}
```

## Why do I get default variation?

- Your flag's kill switch is on
- entity/group entity is in the blacklist
- entity/group entity is not sampled in the population
- entity filters are not matched with the flag filters 