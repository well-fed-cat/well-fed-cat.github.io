---
layout: post
---

### What is Dish?

Dish object may be actually different things depending on the context
or circumstances, in which we consider it.

E.g. one could consider a dish to be a pair of Name and a Recipe (i.e.
description of a way to prepare the dish).

Our system is supposed to be used to compose menus. In this context
a Dish is a "token" or "symbol", which may be inserted into the menu
and represent real life dish. For example it could be name of the dish.
But alternatively the ID of a record from some Dish-Database (or dish-store)
would also fulfil the purpose.

If we consider a dish in context of a dish store, then it should be some
uniquely identifiable piece of data. Different properties can be used
for identifications, such as e.g. its name or some artificial ID.

The examples above illustrate, that depending on which aspect of Dishes
we want to consider in our application, corresponding information should
be included into the Dish object (and even in this case the choise is not
unique).

So in further sections we will try to discuss, which aspects of the Dish
are important for our system and how it will influence the structure of the
object.


### Dish as a part of menu

As we already mentioned, our system is supposed to work with menus, which
are lists of dishes (for now we don't need more specifics). The menus
are supposed to be understood by humans and humans usually recognize the
dishes by their names (note, that even in human's world it is not necesserily
unambigeous).

This means, that to make our system useful for humans, it should generate menus,
which contain human readable dish names. Those names should be more or less
unambigous, but not neccessary fully.


## Equality and inequality of dishes



### Dish as storable object


