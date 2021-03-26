Performance Part 3 


Content Delivery Network

- caches website pages to speed up delivery (if a cached page exists, serve it 
instead of making a request to the server)

- sits between the client and server 

- the server closest to the user serves the resource 

- Cloudflare is the most popular CDN in North America

- requests go through the CDN, which then goes to the server (if necessary)

- Azure has a CDN service

- improves latency


GZIP and Compression

- compresses files (eg. into tar.gz)

- for express, use https://www.npmjs.com/package/compression

- Github Pages does compression automatically

- Apache Web Server settings can set GZIP as well


Brotli by Google

- 20% better compression than GZIP 

https://github.com/google/brotli


Database Scaling 

Ways in which databases can scale:

- identify inefficient queries 

- increase memory 

- vertical scaling (Redis - https://help.compose.com/docs/redis-resources-and-scaling,
Memchached - https://memcached.org/)

- sharding (https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6)

- have more databases 

- use the correct database type for the job!


Identify Inefficient Queries 

- use the databse efficiently 

- don't use needless joins, and only request what is necessary 

- use indexes (sort data - this can make lookup faster using Binary Search, for example). Primary  keys 
are indexed by default, since it's assumed that entries will be queried by the primary key. But,
if we know we will be querying by other fields, like "date", it might make sense to index those fields.


Creating Indexes 

A database index allows a query to efficiently retrieve data from a database.
Indexes are related to specific tables and consist of one or more keys.

CREATE INDEX idx_name ON table_name (column_name);


Vertical Scaling 

- adding a service like Redis so the system can effectivaly use resources 

- by adding another server running Redis that's for memory storage, we have increased 
memory for our database 

    - if a common query is identified, we can cache it, similarly to CDNs (we check if the 
    Redis server already has the data cached)


Sharding 

- break the website / data into different pieces 


More Databases 

- PostGres does this automatically 

- or, you can use a load balancer 

** Mongo has Sharded Cluster Balancers: https://docs.mongodb.com/manual/tutorial/manage-sharded-cluster-balancer/


Caching 

- middleman for cached data 

- caching can occur on the database side (Redis can cache DB data)

- browsers cache data 


Browser caching 

- open Chrome Developer Tools, in the Network tab 

- refresh the React app 

- we can see resources received over the network, and their sizes 

Some resources say 'from ServiceWorker', which means they were cached by the Service Worker.
Others say 'from disk cache', 'from memory cache', etc.

ServiceWorkers intercept HTTP requests, and sees if the data actually needs to be 
requested.


In Chrome Developer Toos, Application tab, we can see our cache, storage, service workers, 
etc. 


Memory vs. Disk Cache 

- similar to in a computer, memory cache (eg. RAM) is limited, but faster to access than 
disk cache 


Cache Busting

- viewers may see an older, cached version of the site (eg. an old CSS file)

- in this case, we need to "bust the cache", and re-download resources from the server 


Webpack does cache busting automatically to prevent this 

Say you have bundled files:

build/static/js/main.305f.js 

build/static/js/main.785b.js 

If we change the CSS, and then run 'build', the name of our bundled CSS files change,
and the name of our bundled JS stays the same! So, the browser uses the new CSS and old 
JS; only updating what has actually changed.

We don't have to worry about implementing Cache Busting. Create-react-app and browsers 
do this (as do Service Workers).


Implementing Caching Ourselves 

- if we control our own server, eg. using Express, we can take advantage of the HTTP cache 
that comes with all browsers 

Request headers come with 2 things that are used for caching:

1) Cache-Control

2) ETag 

Example 

1) Cache-Control: max-age=120 // the amount of time in seconds we want the browser to remember content

2) ETag: "x234dff"

** hard refreshing the browser clears the cache 


If a resource's ETAG matches a cached ETAG, then the resource has not been modified, and 
the cached resource is used.

** In Express, we can also set use res.header() to set headers like Cache-Control,
Content-Type, etc.

** cache-control=0 means we don't want something cached 


Load Balancing 

- handle many requests at the same time and distribute them to different services 

- companies like HostGator and BlueHost run servers for you so that you don't need to 
provide your own 

- these companies use either Apache HTTP server or Nginx for serving static content 

- Apache HTTP server and Nginx simply provide resources based on requests 

- they can be used as load balancers 

** Amazon ElasticBeanstalk supes up EC2 instances, providing extra services like Load 
Balancing. It's used in AllKenora.


SPA -----> Load Balancer (eg. Nginx) --------| ----> Server 1
                                             | ----> Server 2
                                             | ----> Server 3


The Load Balancer acts as a reverse proxy, and can cache requests.


Eventually, you can have a setup where there's a CDN with cached and gzipped versions
of the HTML, CSS, and JS files.

API requests are made to an API endpoint. A Load Balancer is positioned between the SPA and 
the API, and either:

- sends back a cached, gzipped response 

- redirects the request to server 1, 2, or 3, if the response isn't cached 


HTML, CSS, JS

       |
       |
       v 

       GZIP 

       |
       |
       v 

       SPA <---  GZIP  ---> Load Balancer (eg. Nginx) ------------> Server 1
                                                            |-----> Server 2
                                                            |-----> Server 3


** Creating Load balancers yourself is not common practice! Use something like Elastic Beanstalk 
which does it for you.




       
       





