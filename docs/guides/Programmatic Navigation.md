---
title: Programmatic Navigation
---

It is possible to trigger navigation programmatically with the [`push`](../api-reference/route-definition/push.md) and [`replace`](../api-reference/route-definition/replace.md) functions on a route definition.

- `push` will add a new entry to the application's navigation history
- `replace` will take the place of the current entry in history

Programmatic navigation can be used in any number of scenarios but is commonly used for cases such as redirecting from old urls or sending the user to a login page if they haven't yet signed in.
