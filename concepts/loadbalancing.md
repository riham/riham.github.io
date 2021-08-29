<h1>
Load Balancing 
</h1>
Once traffic to your service increases beyond the capacity of a single server, client requests start failing.
That's when we start thinking about distributing the traffic across multiple servers.

This distribution of traffic is what we can load balancing.

Next we explore the mechanisms for load balancing.

<h2>
DNS load balancing
</h2>
This is one of the simplest mechanisms for load balancing. 

To get an idea about how this works, let's talk about the first step when a client issues a request to yourdomain.com.
Yes, exactly .. That would be resolving 'yourdomain,com' to an ip.
Which ip? Well that would be the one you configure at your domain registerer for your domain.

So what exactly is DNS load balancing? It is configuring your domain to point to a set of ips instead of a single one.
How does that work? Whenever a client resolves your domain, DNS servers will send a list of the IPs you configured to the client. Most clients will use the firs IP in the list. However since most DNS by default send the list in different order to each client, this means that each client will be connecting to a differnt sip from the list.

<h3>
So what is cool about it
</h3>
It is very easy to configure.
<h3>
What is not cool about it
</h3>
1- DNS record caching and TTL. Since DNS records are cached, it would take up to the TTL you configured for a change to your DNS to propagate

2- If one of your servers is down or talking too long to respond, DNS doesn't care. Since DNS doesn't check for server load or network errors. It will still send
the server IP to clients.
<h3> 
Taking DNS load balancing to the max: Facebook DNS load balancing 
</h3>
Check out the system Facebook built for dynamically updating its DNS server based on capacity

https://www.youtube.com/watch?v=bxhYNfFeVF4&t=1875s
