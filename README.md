# Goals
==============
* to create a simple flexibile highly-concurrent network transparent library for Clojure
* to identitfy issues with existing actor/dataflow/agent systems and attempt to rectify them
* to look at what other langauges (Scala, Erlang, etc.) do and try to do something better



## Overview of existing methods
==============

### Erlang Actors 

Attributes:
* Actors are processes with premptive multi-tasking
* Communication between actors is via pids
* pids are first class values
* communication is network transparent
* actors are named (via pids)

Complexity/Issues:
* actor state is contained in the closures of the functions being executed
* recv is a blocking operation
* mailboxes are searched, not fifo queues. This means unhandled messages can bloat queues
* connections between actors are implicit, since pids are values therefore they are hidden in function closures

