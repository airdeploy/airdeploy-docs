---
id: version-3.0.x-react
title: React API Reference
sidebar_label: React
original_id: react
---


## FlagProvider

```typescript
interface IFlagProvider {
     apiKey: string 
     sourceURL?: string
     backupSourceURL?: string
     sseURL?: string
     ingestionURL?: string
     logLevel?: 'warn' | 'warning' | 'deb' | 'debug' | 'err' | 'error'
     entity: IEntity // entity to set for all FlagProvider's children
     loadingView: JSX.Element // render a view when Flagger is getting configuration
   }

  defaultProps = {
    children: null,
    loadingView: null,
  }
```

The purpose of this component is to initialize Flagger, `apiKey` is the only required field. 

Pass an entity to share it across all FlagProvider's children:
```javascript
import {FlagProvider, FlagSwitch, Flag, withFlag} from 'flagger/react'

const App = () => (
  <FlagProvider envKey="YOUR_ENV_KEY" entity={user}>
    {// insert rest of app}
  </FlagProvider>
)
```

## Flag

The Flag component renders its children based on whether the case prop matches the flag variation. 
The entity is inherited from FlagProvider if provided; 
if not, make sure to provide a user / entity object to the flag component.

```javascript
<FlagProvider envKey="YOUR_ENV_KEY" entity={user}>
  <Flag case="on" flag="color-theme">
    <NewColorComponent />
  </Flag>
</FlagProvider>
```

## FlagSwitch

FlagSwitch component is useful, especially for multivariate flags that are not simply on/off - 
and have more treatments to handle. 
It's used in combination with the Flag component.
```javascript
<FlagProvider envKey="YOUR_ENV_KEY" entity={user}>
  <FlagSwitch flag="color-theme">
    <Flag case="blue">
      <Button color="blue" />
    </Flag>
    <Flag case="lavender">
      <Button color="lavender" />
    </Flag>
    <Flag case="off">
      <Button color="default" />
    </Flag>
  </FlagSwitch>
</FlagProvider>
```

## withFlag()

withFlag() is a variadic HOC that injects feature flags information as the flags prop into the wrapped component. 
This prop allows you to write more complex conditional logic within a component. 
The prop is a dictionary of flag names to flag info:

```javascript
this.props.flags['color-theme']
// => {
//    enabled: true,
//    eligible: true,
//    treatment: 'on',
//    payload: null,
// }
```

```javascript 
import React, {Component} from 'react'
import {withFlag} from 'flagger/react'

class PaymentOptions extends Component {
	render() {
    if (this.props.flags['color-theme'].enabled) {
    	return <NewColorComponent />
    } else {
      return <DefaultColorComponent />
    }
  }
}

export default withFlag(PaymentOptions, 'color-theme', 'another-flag-name')
```

