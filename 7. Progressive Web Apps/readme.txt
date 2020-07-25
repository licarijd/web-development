Progressive Web Apps 


Lighthouse

- can be used to tell us how progressive our apps are 

- there's a handy Lighthouse Chrome plugin we can use 


** Create React App makes apps progressive out of the box


3 Things Progressive Web Apps Need:

1) HTTPS 

2) App Manifest 

3) Service Worker


HTTPS

- easiest way to get HTTPS is through let's encrypt (Free TLS Certificates:
https://letsencrypt.org/)

- Cloudflare could be used as a hosting service, and like Github pages, automatically uses HTTPS 


Real Favicon Generator

https://github.com/RealFaviconGenerator


Service Worker 

- a script which the browser runs in the background 

- seperate from the webpage 

- generally used for features which don't require a webpage 

- acts as a proxy, and lets us decide what happens on a request by request basis

- can make web apps work offline 

- middleman between web apps and the network

- uses cache API to serve cached pages instead of requesting the same file from the server 

- cache storage and service worker info can be found in the Chrome dev tools 

- serviceWorker.js in React handles this 

- most widely used to view sites offline 