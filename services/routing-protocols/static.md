# Static

A static route is routing information that is manually configured on the routing device.

As the name says, this kind of routing information is fixed in the routing table, so it does not change when there is a networking event or a change to the network.\
Witch means if a problem occurs, and there is another path a packet can be sent, it wont be until a worker manually does not change the route.

***

## Advantages and Limitations

There are some advantages to static routing, and it can and should be used in some scenarios. However, static routing does have its limitations, and this is why [dynamic routing](dynamic/) was developed. &#x20;

For example, static routing uses very little CPU and memory resources, since no algorithms are run to determine the next hop. \
On small networks, where there are two or three routers, there are very few options for what route-specific packets should take. Implementing static routing is more than sufficient in such cases, and is often simpler to implement.

However, as networks get larger, and potential paths to specific destinations become more numerous, static routing becomes more unwieldy to administrate.
