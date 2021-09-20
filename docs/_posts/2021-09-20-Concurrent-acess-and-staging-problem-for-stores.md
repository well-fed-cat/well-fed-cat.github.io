---
layout: post
---

Some important aspect, which needs to be considered, while designing some
store (or even just its interface and functionality) is if it is supposed to
have some staging in any form.

For example, if the object is saved into the DB, but for work in the
application the local object is created, which can be modified and the
"persisted" into the DB, then this object represents "staging".

Another example would be the stateful GUI, which has "Apply" button. E.g.
GUI for modification of Dish could load current properties of the Dish and
then let user change some of the properties and then press "Apply" to
save the changes. Current modified values in the fields are "staged"
changes.

The problem with staging is, that while some changes are staged, but not
yet committed, some other changes may have been committed. Subsequent
commit would either overwrite intermediate changes, which is bad, or
would need to be discarded, which is also bad. The second soluiton is
usually considered preferable, because it allows to at least notice
such an occasion and notify user. For supporting it such mechanisms
as versioning or timestamping could be used.

Note, that the problem originates from concurrent access and
non-atomic/non-blocking nature of update operations, 
and is inherent for any multithreaded systems.
As is usually done in multithreaded systems one could use locking to
overcome this. In case with GUI for exammple it would mean, that user
initiates editing explicitly, e.g. by clicking "edit" button, which
locks the entity for editing for other users. The lock is there and
no one else can edit the entity until user commits or discards the
changes.

One could try to avoid staging and reduce locking.
E.g. in case of the DB and local object
one could make a local object just a shallow thing, which only contains
object ID and all methods go to the DB and each time gets fresh data
from it or even execute the action right in the DB. In case of the GUI
to avoid staging we would need to implement automated updates of the
values in the GUI as soon as the update was performed in the Store
(in similar manner as google-docs allowed simultaneous editing of the
documents).

In our case the main source of "staging"-problem is possibility for user
to edit dishes. Editing time is naturally not too small, so the possibility
of concurrent editing of the same dish by multiple users is also not small.

For now we do the design assuming, that the staging exists and particularly,
that local objects contain information, which can actually be outdated.
To support check during saving, version will be added to Dish objects.
For now backend (store) in case of conflict will just fail the update
operation for the changes, which come later.

Later to improve user experience different techniques can be employed,
e.g. locking of the dishes while editing or callback, which would notify
user about external changes with possibility to update current state.
