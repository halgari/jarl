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
* actors can transform by providing a fn to process the next message for that actor

Complexity/Issues:
* actor state is contained in the closures of the functions being executed
* recv is a blocking operation
* mailboxes are searched, not fifo queues. This means unhandled messages can bloat queues
* connections between actors are implicit, since pids are values therefore they are hidden in function closures

### Clojure Agents

Attributes:
* agents contain state
* state can be retreived via a non-blocking deref call
* messages are sent as pairs of functions and arguments
* message functions transform the old state to a new state
* agents are not named (there is no global lookup of all agents)

Complexity/Issues:
* agents must be sent functions, therefore the sender must be aware of program logic, not the receiver
* agents are not network enabled
* even if clojure fns were serlialized, these methods would most likely not work in ClojureScript (fns are not cross-platform)


### Dataflow

Attributes:
* consists of nodes connected via "pipes"
* each node can have one or more inputs and one or more outputs
* pipes can be views as queues, and can duplicate messages to multiple nodes
* most often, nodes are named
* node relationships are explicit, a connect call must be made to associate two nodes

Complexity/Issues:
* To connect two nodes, one must know both the output port of one node and the input of annother
* Most systems to not define a clear aggregation method: multiple outputs cannot feed into the same pipe



## Our method

Attributes:
* Pipes are globally named
* Nodes are values
* Anonymous pipes are allowed (as not to polute the global pipe namespace)
* Nodes have uniquely named I/O ports
* Each port can either be input or output not both
* Connections are therefore between pipes and ports
* Data sent into a pipe is duplicated to all output ports
* Pipes are queues
* Pipes are network transparent (i.e. you can connect to a pipe on a different machine)


**** more thoughts to follow ****
