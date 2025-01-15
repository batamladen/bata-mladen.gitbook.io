---
description: 'Metric: Hop Count'
---

# Distance Vector

All routing protocols use an algorithm or set of algorithms to calculate and distribute routing information.&#x20;

A distance vector protocol (DVP) is a dynamic routing protocol that uses an algorithm based on the work of Bellman, Ford, and Fulkerson known as **vector distance** or the Bellman-Ford algorithm.&#x20;

Distance Vector protocols are:

* [RIP](rip.md)
* [IGRP](igrp.md)

The idea behind the DVP algorithm is that each router on the network (routing domain or process) compiles a list of the networks it can reach and sends the list to their directly connected neighbors. \
The routers then create routing tables based on what they can reach directly and indirectly, using their neighbor routers as gateways. If multiple paths exist, the router only keeps the best one. This route is chosen based on the protocol's particular metric for determining the best route. Routing protocols each use different route metrics to choose the best route.

***

## Pros & Cons

### <mark style="color:green;">Pros:</mark>

**Distance Vector Routing Protocols** offer simplicity and ease of implementation, making them ideal for smaller networks or those with limited administrative expertise. These protocols require less router resources, making them suitable for hardware-constrained devices.&#x20;

### <mark style="color:red;">Cons:</mark>

However, they suffer from slower convergence times compared to[ link-state protocols](../link-state/), as routing updates are exchanged periodically and must propagate throughout the network. Additionally, their reliance on hop count as the primary metric can lead to routing loops and scalability challenges in larger and more complex networks. Despite these limitations, distance vector protocols remain a viable option for simple network setups where ease of configuration outweighs scalability concerns.



<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><mark style="color:green;"><strong>Pros:</strong></mark></td><td><ul><li>Simplicity</li><li>Ease of Implementation</li><li>Low Router Resource Consumption</li></ul></td></tr><tr><td><mark style="color:red;">Cons:</mark></td><td><ul><li>Slow Convergence</li><li>Routing Loops</li><li>Count-to-Infinity Problem</li></ul></td></tr></tbody></table>
