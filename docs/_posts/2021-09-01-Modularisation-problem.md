---
layout: post
---

### Modularization problem

As of now (2021-07-31) well-fed-cat has already multiple sub-components. Currently
they are all maintained in the same repository, which makes it not easy to apply
significant changes to the core components (lots of adjustments need to be done in
the dependent components, which cannot be done at once).

The solution, which comes to mind is to split the whole system into multiple 
repositories, so
that some components could be first developed or modified independently and then
other parts of the system could accomodate the changes.

But unfortunately this approach is not easy to apply cleanly.

The first solution, which came to my mind was:

* We make multiple nested submodule repositories. Each outer one contains those
  components on which it depends as submodules. 
  E.g. "datamodel" does not have any dependencies
  (by the way it is not true), and datamodel-db-h2 contains then "datamodel" as
  submodule. And then furhter particular component contains datamodel-db-h2 as
  its submodule. This way all "inner" components can be implemented independently,
  while all outer components have a possibility to pinpoint particular version
  of the inner submodule (and recursively of all nested submodules).

This approach though has multiple weaknesses:

* The structure of the gradle "products" would be not flat, but quite ugly
  nested.
* Much more importantly: the graph of dependencies (even when well formed)
  is not really a tree and thus it cannot be represented by nested components.
  E.g. two dependencies may depend
  on the same component, so nested component would be repeated. Besides, the current
  component itself could be dependent on the same sub-component (should it include
  this sub-component again, or use one of its "bigger" dependencies? which one?).

The second problem seems to be fundamental and still not be solved. Basically
"dependency hell" is the manifestation of this problem. And e.g. Maven management
of the transient dependencies is one of they attempts to solve it. In my
opinion it does not really properly solve this problem, but I also don't
know fundamentally different and better solution.

To come around this problem I think we can give up on achieving "generic-flexibility",
i.e. that each component can be maintained independently from others except it's
explicit and trainsient dependencies (but not considering components, which in turn
depend on it).

Instead we will split components into two groups (or probably later more):
* core components:
  * utilities
  * datamodel
  * datamodel-test
  * core (should be renamed to planning-algorithms or something like this)
* store implementations:
  * in-memory store
  * in-file store
  * h2-db-store

Components from the first group will be combined into single repo.

Components from the second group will have each it's own repo having "core-components"
as submodule.

There will be one more repo, which has set of scripts to manage and build all
"specific-implementation" repos, so that one could check, if all are functionall
(not really yet sure, how exactly to acheive this).

There is one further layer of components: UI. E.g. interactive-console, desktop,
web etc. Theoretically they can be arbitrarily combined with the stores
implementation, which makes the task of managing all this system quite
complicated. Probably we just need to select couple of specific combinations
and just fucuse on those...
