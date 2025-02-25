---
description: 'Metric: Cost'
---

# Link-State

## What is Link-State?

Link state protocols (LSPs) are based on a different algorithm than DVPs. LSPs are based on a graph theory algorithm called Dijkstra's algorithm, named after its creator E.W. Dijkstra.&#x20;

The algorithm works like this:\
The premise is that a collection of points is seen as a tree; one point is the base and the shortest path to all other points as the pathway. The cost of the path is the sum of the path costs between the source point and the terminus point. Whereas DVPs are interested in the shortest route, LSPs are interested in the operational status of all of the links in the network. Each host compiles a "map" of the routing domain from its own point of view.

Link-State protocols are:

* [OSPF](ospf.md)
* [ISIS](is-is.md)

***

## Pros & Cons

### <mark style="color:red;">Cons:</mark>

Link-State's limiting aspect is the memory required by the router to compile the shortest path distances needed for the routing table. On a small network consisting of 20 or so routers, these computations are manageable, but on a network of 100 or more, these calculations can cripple a small router with limited CPU and memory. \
LS Protocols are complex which can make them difficult to troubleshoot, and there is a problem with flooding[^1].

### <mark style="color:green;">Pros:</mark>

On the other hand, LS protocols have fast convergence time because each router has its own complete map of the network. They have more efficient usage of network resources because they only send updates when there is a change in the network topology, reducing the amount of traffic.\
Also, LS protocols can handle large networks.



<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><mark style="color:green;"><strong>Pros:</strong></mark></td><td><ul><li>Fast Convergence</li><li><strong>Efficient Resource Usage</strong></li><li><strong>Scalability</strong></li></ul></td></tr><tr><td><mark style="color:red;"><strong>Cons:</strong></mark></td><td><ul><li><strong>Memory Requirements</strong></li><li><strong>Complexity</strong></li><li><strong>Flooding Issues</strong></li></ul></td></tr></tbody></table>



[^1]: Flooding is a common issue with link state protocols. It occurs when a link state advertisement (LSA) is transmitted by a router and forwarded by other routers to all their neighbors, including the originating router.
