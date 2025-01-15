# OSPF

**Open shortest path first** (OSPF) is a **link-state routing protocol** that is used to find the best path between the source and the destination router using its own shortest path first (SPF) algorithm. A link-state routing protocol is a protocol that uses the concept of triggered updates, i.e., if there is a change observed in the learned routing table then the updates are triggered only, not like the distance-vector routing protocol where the routing table is exchanged at a period of time.&#x20;

In an OSPF protocol, routers have different roles. Whitch are:

1. **Backbone router –** The area 0 is known as backbone area and the routers in area 0 are known as backbone routers. If the routers exists partially in the area 0then also it is a backbone router.
2. **Internal router –** An internal router is a router which have all of its interfaces in a single area.
3. **Area Boundary Router (ABR) –** The router which connects backbone area with another area is called Area Boundary Router. It belongs to more than one area. The ABRs, therefore, maintain multiple link-state databases that describe both the backbone topology and the topology of the other areas.
4. **Area Summary Border Router (ASBR) –** When an OSPF router is connected to a different protocol like EIGRP or Border Gateway Protocol, or any other routing protocol then it is known as AS. The router which connects two different AS (in which one of the interfaces is operating OSPF) is known as Area Summary Border Router. These routers perform redistribution. ASBRs run both OSPF and another routing protocol, such as RIP or BGP. ASBRs advertise the exchanged external routing information throughout their AS.

{% hint style="info" %}
**Note:**

A router can be backbone router and Area Boundary Router at the same time i.e a router can perform more than one role at a time.
{% endhint %}

***

## Commands:

