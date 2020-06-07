---
title: param
---

The `param` object provides an ergonomic way to access the parameter definitions used to define which parameters a specific route takes. Here's a list of all parameter definitions:

- `param.path.string`
- `param.path.number`
- `param.path.boolean`
- `param.path.ofType(valueSerializer)`
- `param.path.optional.string`
- `param.path.optional.number`
- `param.path.optional.number.default(value)`
- `param.path.optional.boolean`
- `param.path.optional.boolean.default(value)`
- `param.path.optional.ofType(valueSerializer)`
- `param.path.optional.ofType(valueSerializer).default(value)`
- `param.path.trailing.string`
- `param.path.trailing.number`
- `param.path.trailing.boolean`
- `param.path.trailing.ofType(valueSerializer)`
- `param.path.trailing.optional.string`
- `param.path.trailing.optional.string.default(value)`
- `param.path.trailing.optional.number`
- `param.path.trailing.optional.number.default(value)`
- `param.path.trailing.optional.boolean`
- `param.path.trailing.optional.boolean.default(value)`
- `param.path.trailing.optional.ofType(valueSerializer)`
- `param.path.trailing.optional.ofType(valueSerializer).default(value)`
- `param.query.string`
- `param.query.number`
- `param.query.boolean`
- `param.query.ofType(valueSerializer)`
- `param.query.optional.string`
- `param.query.optional.string.default(value)`
- `param.query.optional.number`
- `param.query.optional.number.default(value)`
- `param.query.optional.boolean`
- `param.query.optional.boolean.default(value)`
- `param.query.optional.ofType(valueSerializer)`
- `param.query.optional.ofType(valueSerializer).default(value)`
- `param.state.string`
- `param.state.number`
- `param.state.boolean`
- `param.state.ofType(valueSerializer)`
- `param.state.optional.string`
- `param.state.optional.string.default(value)`
- `param.state.optional.number`
- `param.state.optional.number.default(value)`
- `param.state.optional.boolean`
- `param.state.optional.boolean.default(value)`
- `param.state.optional.ofType(valueSerializer)`
- `param.state.optional.ofType(valueSerializer).default(value)`

These are organized first by parameter position (path, query, state), then by any parameter modifiers (optional, default, array, trailing), and finally by the parameter type (string, number, boolean, ofType).

## Parameter Position

### Path

Path parameters are found in the path of the url. Given this url `https://typehero.org/type-route/example/abc?page=1` the path would be this segment `/type-route/example/abc`. A `string` path parameter could be used to parameterize the `abc` part of this path.

### Query

Query parameters are found in the query string of the url. Given this url `https://typehero.org/type-route/example/abc?page=1` the query string would be this segment `?page=1`. A `number` query parameter could be used to parameterize the `page=1` part of this query string.

### State

State parameters are found in the browser's history. They are invisible to a user. Data too large or cumbersome to put into a url that should still be properly persisted and restored with the browser's forward/back buttons could be stored here.

## Parameter Modifiers

### Optional

The optional modifier will ensure that, even if a particular parameter has not been provided a value, the route will still be able to be successfully matched. The optional modifier may only be used for a path parameter if the parameter is at the end of the path.

### Default

Any `optional` parameter may also provide a `default` modifier. The default modifier will ensure that even if a value was not provided for a particular parameter it will
appear to the application that a value was provided. Paginated list pages could benefit from this modifier, for example, to guarantee that a query parameter specifying the `page` has the default value 1 if not other value is provided.

### Array

The `array` modifier will take the parameter type given and modify it to be an array.

### Trailing

The `trailing` modifier may only be used with path parameters. It is used to designate a parameter that is a catch all or wildcard. It may only be used as the last parameter in a path. Even if the value is unused by the application this modifier can ensure that paths will match as long as the beginning portions are the same.

## Parameter Types

### Number

Only values that can be parsed to a number will match this parameter.

### String

All values are parsable to a string so every value will match the string parameter.

### Boolean

Only values that can be parsed to a boolean will match this parameter.

### ofType

When the default parameter types used in conjunction with modifiers such as `array` are unable to meet your needs the `ofType` function gives you an escape hatch to express any type for which you are able to write a serializer/deserializer.

```tsx
const date: ValueSerializer<Date> = {
  parse(raw) {
    const value = Date.parse(raw);
    if (isNaN(value)) {
      return noMatch;
    }
    return new Date(value);
  },
  stringify(value) {
    return value.toISOString();
  }
}

defineRoute(
  {
    dateAddedFilterMin: param.query.optional.ofType(date),
    dateAddedFilterMax: param.query.optional.ofType(date)
  },
  () => `/users`
)
```

It is also possible to not specify a value serializer for the `ofType` function and only provide a generic type parameter. In this case Type Route will use the default json serializer with the object you provided. In most cases this is not recommended because you have no guarantees that the json parsed will be of the shape you expect. Where this is used there should be application code in place to gracefully handle conditions where data is not of the expected type.

```tsx
type SomeSuperComplexJsonYouDoNotWantToWriteASerializerFor = { ... };
param.query.ofType<SomeSuperComplexJsonYouDoNotWantToWriteASerializerFor>();
```