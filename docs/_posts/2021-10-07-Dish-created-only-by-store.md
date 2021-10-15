---
layout: post
---

As discussed in another note, we would like to enforce
the fact, that only store can create a Dish. Unfortunately it seems, that
we cannot really acheive it with only Dish design (i.e. with the design
of well-fed-cat-core-components). This can only be enforced by
the implementation of a particular store. And because core-components do
not controll implementation of particular stores, we can only articulate
in the documentation conventions, which have to be maintained by
store implementation for he system to work properly.

In particular, we are only interested in Dishes, which are considered
in context of DishStore, so if DishStore simply does not provide the
possibility to store existing dishes created outside of the store, then
those dishes created outside of the store become useless.


## Ideas, that do not work

It is obvious, that we cannot make constructor of Dish private, because
at least DishStore needs to create Dish objects.

At the beginning I has some ideas to fiddle with nested classes or with
package-private methods etc, but it all does not work or makes things too
cumbersome.

After all if we provide possibility to DishStore to create Dishes, then
it can always be used as a jail-break, i.e. one can impleemnt interface
with all dummy methods and only one method implemented, which just fast-
forwards constructor of the Dish. So further thinking about enforcement
of this seems to be non-constructive. We have to just stay with declaring
in the documentation requirements to the compliant DishStore implementations
and probably providing automated tests, which would at least partially
test this compliance.
