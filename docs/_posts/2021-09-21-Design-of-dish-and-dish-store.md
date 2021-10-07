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
be included into the Dish object (and the choise is not unique).

So in further sections we will try to discuss, which aspects of the Dish
are important for our system and how it will influence the structure of the
object or objects.


### Dish as a part of menu

As we already mentioned, our system is supposed to work with menus, which
are lists of dishes (for now we don't need more specifics). The menus
are supposed to be understood by humans and humans usually recognize the
dishes by their names (note, that even in human's world it is not necesserily
unambigeous).

This means, that to make our system useful for humans, it should generate menus,
which contain human readable dish names. Those names should be more or less
unambigous, but not neccessary fully.


### Dish as storable object

In our system the dishes will be stored in a dish-store. The exact
implementation of particular dish-store is not important and we will try
to abstract it out into some interface.

One can also make different assumptions about the dishes and the store.
E.g. one could allow or prohibit storing the same dish multiple times.
These questions will be discussed in the section devoted to the
DishStore. But even the notion of "being equal" is not smnple and
will be discussed in next section.


### Equality and inequality of dishes

What defines "identity" and "equality" of dishes? One could say, that
equality of dishes means, that all fields of the object shoould be the equal.
Or that it should just be the same object. Alternatively dish could have
some id and we may consider two objects equal, if the IDs are equal.

#### Strong ID

Indeed, our dish will have some global ID (strong ID). And we will try
to shape our system in such a way, that it is impossible, that there are
two objects, which represent different dishes, that have same strong-IDs.
Or more strictly, that if two dishes have the same strong ID, then they
should have the same field values.

To acheive this, the following conditions should be acheived:

* It should be impossible to control strong-ID of the Dish object. Instead
  it should be constrolled by the system itself. This could be acheived,
  by delegating the role of creating dishes to dish-store.

* Additional problem caueses "staging" (see corresponding section). The
  problem is, that we may have two objects, which correspond to the
  different versions of the object. I believe, that this problem cannot
  be fully fixed (at least without having a way to get notifications from
  the DB, when some dish was modified). But this problem can be addressed
  and it's negative consequences may be significantly reduced or even
  eliminated by using the following additional ideas:

  * We should eliminate possibility to use "dirty" objects (i.e. those,
    which were modified, but not yet persisted in the store). To acheive
    this we can enforce, that the modification can only be done by a
    store itself. For this we would introduce an object, which describes
    desired change and store operation, which applies this change to
    give dish specified either by dish object or by dish strong id.

  * Once this is done, we guarantee, that the objects with the same
    strong id and version must be equal. It allows us to compare for
    equality by just using strong ids, assuming, that the versions
    are the same. From the domain point of view having two objects
    of the same dish with different versions in the same context is
    a logical error. This should never happen. As such to enforce it
    we may implement method `equal()` in such a way, that it throws,
    if two objects have same strict id and different versions.

  * These two points provide some ideas, how to avoid exception described
    in previous point:

    * Most important is, that one should not get to details of the dish,
      unless it is really needed. E.g. In menu the dishes can be represented
      just by their strong-ids, and only, when one needs to show the menu
      to the user, then actual names are loaded from the DB.

    * Dish objects (except strong-ids) should be short-living. I.e. they should
      be retreived from the DB for the specific task and then dismissed as
      soon as this task is completed.

#### Name

What about dish name? Is it possible to have two dishes with different
strong IDs but with the same names? This is not desirable, because
from our every-day live we assume, that omlet is an omlet etc.
To facilitate this we will prohibit to have two different dishes with
the same name in the store. On the other hand, if one creates the dish
"omlet" and then deletes it, it should still be possible to create
"omlet" again, and we cannot enforce, that it's definition is the same
(the object will definitely have different "strong-id"). In this sense
the name is not "strong" identifier (unlike strong-id, as also the name
suggests).

Strong-id in constrast is designed in such a way, that it is very unlikely
to be ever repeated for two different dishes even if they are created in
two different stores. And even if hte dish was deleted, it's strong-id
cannot be reused in the same store.


### Staging of dish objects

Staging is discussed in separate document. Here we will mention only, that
we will allow staging. To deal with concurrent access we will employ versioning
and use a rule, that the "first" change always wins. All subsequent changes,
which are made based on "old" version are not allowed to be saved.


### Conclusion

The Dish object will have the following properties (for now):

* name - to make humans recognize it
* id - (was not discussed) - special name to use in interactive console
* strong-id - globally unique identifier of the dish
* version - way to deal with staging and concurrent access
* ??? - way to notice, that the object was modified, but not saved
