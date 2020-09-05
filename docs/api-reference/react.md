---
id: react
title: React API Reference
sidebar_label: React
---

## FlagProvider

```typescript
type IFlagProviderProps = {
  apiKey: string
  sourceURL?: string
  backupSourceURL?: string
  sseURL?: string
  ingestionURL?: string
  logLevel?: 'warn' | 'warning' | 'deb' | 'debug' | 'err' | 'error'
  entity?: IEntity // entity to set for all FlagProvider's children
  loadingView: JSX.Element // render a view when Flagger is fetching configuration
} & (
  | {
      loadingView: JSX.Element
      children: React.ReactNode
    }
  | {
      children: (props: IFlaggerCtx) => React.ReactNode // pass children function for even more control
    }
)

defaultProps = {
  children: null,
  loadingView: null,
}

interface IFlaggerCtx {
  entity?: IEntity
  config?: IFlaggerConfiguration
  loading: boolean
  getVariation: (codename: string, entity?: IEntity) => string
  getFlagDetails: (
    codename: string,
    entity?: IEntity
  ) => {variation: string; enabled: boolean; isSampled: boolean; payload: any}
}
```

The purpose of this component is to initialize Flagger in your application component tree. `apiKey` is the only required field.

Pass an entity to share it across all FlagProvider's children, for example, if there a single user entity that is constant. You can also override this entity with downstream components.

```javascript
import {FlagProvider, FlagSwitch, Flag, withFlag} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
    // insert the rest of the app
  </FlagProvider>
)
```

`FlagProvider` takes a function as a child with parameters passed to it from Flagger.

```javascript
import {FlagProvider, FlagSwitch, Flag, withFlag} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
    {({loading}) => {
      if (loading) {
        return <AppLoading />
      }
      return <AppContent />
    }}
  </FlagProvider>
)
```

## Flag

The Flag component renders its children based on whether the case prop matches the flag variation.

The entity is inherited from FlagProvider if provided; if not, make sure to provide a user / entity object to the flag component.

The props that the Flag component takes are described by the following interface:

```typescript
interface IFlagProps {
  flag?: string
  case: string
  children?: ((props: {case: string; flag?: string}) => React.ReactNode) | React.ReactNode
  component?: React.ComponentType<{
    case: string
    flag?: string
  }>
  entity?: IEntity
  isSwitchChild?: boolean
}
```

The Flag component can be added anywhere in the React component tree as long as it's inside of the FlagProvider.

```javascript
import {FlagProvider, Flag} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
    <Flag case="on" flag="color-theme">
      <NewColorComponent />
    </Flag>

    {/* function as a child */}
    <Flag case="on" flag="color-theme">
      {() => <NewColorComponent />}
    </Flag>

    {/* component prop */}
    <Flag case="on" flag="color-theme" component={NewColorComponent} />
  </FlagProvider>
)
```

## FlagSwitch

The FlagSwitch wraps multivariate flags that are not simply on/off - and have multiple variations to handle. It's used in combination with the Flag component.

The props that the FlagSwitch component takes are described by the following interface:

```typescript
interface IFlagSwitchProps {
  flag: string
  entity?: IEntity
  children?: ((props: IComponentProps) => React.ReactNode) | React.ReactNode
  component?: React.ComponentType<IComponentProps>
}

interface IComponentProps {
  flag: string
  entity?: IEntity
  variation: string
  loading: boolean
}
```

```javascript
import {FlagProvider, FlagSwitch, Flag} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
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

    {/* component prop */}
    <FlagSwitch
      flag="color-theme"
      component={function RenderColorThemeSwitch({variation}) {
        switch (variation) {
          case 'blue':
            return <Button color="blue" />
          case 'lavender':
            return <Button color="lavender" />
          case 'off':
          default:
            return <Button color="default" />
        }
      }}
    />

    {/* function as a child */}
    <FlagSwitch flag="color-theme">
      {({variation}) => {
        switch (variation) {
          case 'blue':
            return <Button color="blue" />
          case 'lavender':
            return <Button color="lavender" />
          case 'off':
          default:
            return <Button color="default" />
        }
      }}
    </FlagSwitch>
  </FlagProvider>
)
```

## Variation

You also can use the `Variation` component (instead of `Flag`) inside `FlagSwitch`. It is used to guard the component sub-tree behind each variation. `Variation` components must be used inside a `FlagSwitch`.

```typescript
interface IVariationProps {
  case: string
  flag?: string
  children?: ((props: {case: string; flag?: string}) => React.ReactNode) | React.ReactNode
  component?: React.ComponentType<{
    flag?: string
    case: string
  }>
}
```

```javascript
import {FlagProvider, FlagSwitch, Variation} from 'flagger-react'

const App = () => (
  <FlagProvider apiKey="<API_KEY>" entity={user}>
    <FlagSwitch flag="color-theme">
      <Variation case="red">
        <Button color="red" />
      </Variation>

      {/* function as a child */}
      <Variation case="green">{() => <Button color="green" />}</Variation>

      {/* component prop */}
      <Variation case="blue" component={BlueButton} />

      <Variation case="off">
        <Button color="default" />
      </Variation>
    </FlagSwitch>
  </FlagProvider>
)
```

## withFlag()

withFlag() is a variadic HOC that injects feature flags information as the flags prop into the wrapped component.

This prop allows you to write more complex conditional logic within a component. The prop is a dictionary of flag names to flag info:

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
import React from 'react'
import {withFlag} from 'flagger-react'

function PaymentOptions(props) {
  if (props.loading) {
    return <Spinner />
  }
  if (props.flags['color-theme'].enabled) {
    return <NewColorComponent />
  } else {
    return <DefaultColorComponent />
  }
}

export default withFlag(PaymentOptions, 'color-theme', 'another-flag-name')
```

## Hook: useVariation()

The hook `useVariation`, like the `FlagSwitch` component, returns the variation for an entity.

```typescript
type useVariation = (codename: string, entity?: IEntity) => IUseVariationResponse

interface IUseVariationResponse {
  codename: string
  entity?: IEntity
  variation: string
  loading: boolean
}
```

```javascript
import {useVariation} from 'flagger-react'

const App = () => {
  const {loading, variation} = useVariation('color-theme')
  if (loading) {
    return <Spinner />
  }
  switch (variation) {
    case 'blue':
      return <Button color="blue" />
    case 'lavender':
      return <Button color="lavender" />
    case 'off':
    default:
      return <Button color="default" />
  }
}
```

It does the same thing as a FlagSwitch component used like this:

```javascript
import {FlagSwitch} from 'flagger-react'

const App = () => (
  <FlagSwitch flag="color-theme">
    {({loading, variation}) => {
      if (loading) {
        return <Spinner />
      }
      switch (variation) {
        case 'blue':
          return <Button color="blue" />
        case 'lavender':
          return <Button color="lavender" />
        case 'off':
        default:
          return <Button color="default" />
      }
    }}
  </FlagSwitch>
)
```

## Hook: useFlag()

The hook `useVariation`, like the `withFlag` HOC, returns details about the flag for handling.

```typescript
type useFlag = (codename: string, entity?: IEntity) => IUseFlagResponse

interface IUseFlagResponse {
  loading: boolean
  codename: string
  entity?: IEntity
  enabled: boolean
  isSampled: boolean
  variation: string
  payload: any
}
```

```javascript
import {useFlag} from 'flagger-react'

const App = () => {
  const {loading, enabled, isSampled, variation, payload} = useFlag('color-theme'/*, entity */)
  if (loading) {
    return <Spinner />
  }
  ...
}
```

It does the same thing as the `withFlag` component used like this:

```javascript
import {withFlag} from 'flagger-react'

const WrappedComponent = ({flags, loading}) => {
  const {enabled, isSampled, variation, payload} = flags['color-theme']
  if (loading) {
    return <Spinner />
  }
  ...
}

export default withFlag(WrappedComponent, 'color-theme')
```
