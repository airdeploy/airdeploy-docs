---
id: version-3.0.0-attributes
title: Attributes
sidebar_label: Attributes
original_id: attributes
---

Both entity and group entity could have attributes. These are key-value pairs provided by the developer.
`Flagger` does some attributes manipulations before trying to match it against filters.

Here is the wrap-up of all the things `Flagger` does with attributes:

1. `Flagger` lower case all the keys in the Entities attributes:


    {"IsAdMiN": true} 
becomes
 
    {"isadmin": true}

---
2. `Flagger` copies `id` and `type` to the filter attributes unless `id` or `type` is defined in attributes, 
examples:


    {id:"1", type:"User"} 
becomes
 
    {id:"1", type:"User", attributes:{id:"1", type:"User"}}

But does not override provided attribute
    
    {id:"1", type:"User", attributes:{id:"2"}}
notice that `id` stays untouched
 
    {id:"1", type:"User", attributes:{id:"2", type:"User"}}

---
3. Both `is_not` and `not_in` filter returns true if there is no attribute with that key, examples:
  
  
    //filter: age "is_not" 20
    {id:"1", type:"User", attributes:{id:"1", type:"User"}} // true, since there is no attribute "age"
    
    //filter: age "not_in" [20]
    {id:"1", type:"User", attributes:{id:"1", type:"User"}} // true, since there is no attribute "age"

---

4. Golang's users must use only `int` or`float64` types for number types.  

```go
&core.Entity{ID: "1", Attributes: map[string]interface{}{
		"age":         21, // int
		"friends":     55, // int
		"probability": 0.5, // float64
	}}
```