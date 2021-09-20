# Well Fed Cat

This website is not about "Well Fed Cat" and not even about
its development. It is rather just a bunch of notes, which I made
for myself.

Well-Fed-Cat on GitHub: [https://github.com/well-fed-cat](https://github.com/well-fed-cat)



## TODO ##

* Continue work on refactoring of "datamodel" module.
* Repair SQL-DB implementation.
* Complete test-suite for datamodel and data stores and make sure,
  it works for in-memory implementation.
* Make sure, all tests work for SQL-DB implementation.
* Learn about gradle and java modules.
* Extract in-memory implementation into separate repo. Make multi-submodule common.
* Repair in memory implementation
* Extract simple-file implementation as separate repo.
* Repair simple-file implementation and make sure, all tests work.
* Implement automated tests.
  * Implement them for all store-implementations.
  * Create spec of how stores should work together.
  * Make store implementations compliant with the spec.
  * Fix problem with over-writing DayMenus in MenuTimelineStore
* Improve interactive work:
  * For all datamodel classes implement meaningful toString()
  * Implement dishStore.findById("textToSearch")
* During generation of new menu the algorithm should take into account
  the saved history (probably depth can be specified).
* Add "basic probability coefficient" to the dish to make it possible 
  for user to make some dishes more frequent, than another one.
* Add flag to dish to exclude it from using in menus.
* Add flag to disallow dish to be used, more than one time within the week.
* Add string tags to dish. Reduce probability of dishes with same tag (if used recently).
* Add field to define allowed days of week for the dish (mainly for "weekend-dishes").
* Add flag "deleted" to dishes in the DB (do we need to propagate it also
  to the objet, or we should do all the filtering on the "query" stage?),
  so that we can delete some dish from the store, but still show it in the
  menu timeline. Alternatively the flag can be "timeline-only".
* Test MenuTimelineStoreSimpleFile (probably interactively).
* Add api to modify dish (`update()` method, which replaces dish object
  by ID with provided object).
* Add api to modify dish public ID.
* Maybe we need some tools to clean up the history.
* Add automated tests.
* Make DB-based dishStores check schema version.
* Try to use java modules
* Try to properly encapsulate implementation details and restrict visibility.
* Make Javadoc combined from all submodules.
