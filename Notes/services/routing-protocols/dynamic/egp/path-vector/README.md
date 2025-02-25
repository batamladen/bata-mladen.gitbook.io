# Path Vector

A **path-vector routing protocol** is a network[ routing protocol ](../../../)which maintains the path information that gets updated dynamically. Updates that have looped through the network and returned to the same node are easily detected and discarded. This algorithm is sometimes used in Bellman–Ford routing algorithms to avoid "Count to Infinity" problems.

It is different from the[ distance vector routing](../../igp/distance-vector/) and [link state routing](../../igp/link-state/). Each entry in the routing table contains the destination network, the next router and the path to reach the destination.

The only Path Vector protocol is:

* [BGP](bgp.md)

