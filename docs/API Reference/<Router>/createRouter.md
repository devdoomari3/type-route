---
title: createRouter
---

```tsx
createRouter(routeDefinitions: RouteDefinitionBuilderCollection): Router
```

Initializes a router. By default the underlying history instance which powers Type
Route will be configured according to the environment your code is running in. This
means Type Route should work out of the box in both browser and non-browser environments
such as React Native. You can always reconfigure the history instance you are given to
cover other use cases (such as server-side rendering).

#### Example

```tsx
const { routes, listen, getCurrentRoute, history } = createRouter({
  home: defineRoute("/"),
  postList: defineRoute(
    {
      page: "query.param.number.optional"
    },
    p => `/post`
  ),
  post: defineRoute(
    {
      postId: "path.param.string"
    },
    p => `/post/${p.postId}`
  )
});
```

`createRouter` will create a `Router` object. Immediately destructuring this `Router` object into the properties your application needs is the recommended style.
