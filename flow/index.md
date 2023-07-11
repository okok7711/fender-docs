---
layout: default
title: Flow
nav_order: 2
has_children: true
permalink: docs/flow
---

### [`<T|R> if(cond: Bool, if_true: T|Function<T>, if_false: R|Function<R>)`](./if.md)
### [`T else(cond: Bool, body: T|Function<T>)`](./else.md)
### [`<Null|Bool> then(cond: Bool, body: Function)`](./then.md)
### [`Bool while(cond: Function, body: Function)`](./while.md)
### [`T also(incoming_val: T, func: Function<T>)`](./also.md)
### [`T apply(incoming_val: T, func: Function<T>)`](./apply.md)
### [`T onErr(incoming_val: T, func: Function<T>)`](./onError.md)
### [`T onNull(incoming_val: T, func: Function<T>)`](./onNull.md)
### [`T onType(incoming_val: T, type_id: R, func: Function<T>)`](./onType.md)
