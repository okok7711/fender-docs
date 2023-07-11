---
layout: default
title: Flow
nav_order: 2
has_children: true
permalink: docs/flow
---
## Control Flow Functions

### [`<T|R> if(cond: Bool, if_true: T|Function<T>, if_false: R|Function<R>)`](../flow/if)
### [`T else(cond: Bool, body: T|Function<T>)`](../flow/else)
### [`<Null|Bool> then(cond: Bool, body: Function)`](../flow/then)
### [`Bool while(cond: Function, body: Function)`](../flow/while)
### [`T also(incoming_val: T, func: Function<T>)`](../flow/also)
### [`T apply(incoming_val: T, func: Function<T>)`](../flow/apply)
### [`T onErr(incoming_val: T, func: Function<T>)`](../flow/onError)
### [`T onNull(incoming_val: T, func: Function<T>)`](../flow/onNull)
### [`T onType(incoming_val: T, type_id: R, func: Function<T>)`](../flow/onType)
