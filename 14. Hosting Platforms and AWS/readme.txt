Hosting Platforms and AWS 


Top 3 Platforms:

- AWS 

- Google CLoud Platform

- Microsoft Azure 


These platforms can do things like database scaling, load balancers, servers, etc.


Amazon AWS provides:

- CDNs 

- Database 

- Storage 

- and lots of other services! See the AllKenora dashboard 


Most Common AWS Services 


EC2

- most basic Linux server


s3

- object storage service 

- every object is stored as a file 

- every object corresponds to a key 

- enables us to load/store practically any object (images, files, etc)


Lambda 

- runs functions 

- can schedule running the code 

- handles creating instances of functions 

- can have events that are triggered 


Cloudfront (CDN)

- content, like images and files, can be put here 

- Amazon manages delivery 

- provides automatic HTTPS


DynamoDB 

- NoSQL database 

- manages scaling 

- key-value storage model 

Check out 'AWS flow' on Google to see how people do things! This is devops.


Microservices Architecture 

- splitting application into smaller, interconnected services, unlike large monolithic 
services 

- each mocroservice is a small application with it's own architecture and business logic,
concerned with doing one thing very well


Serverless Applications 

- rather than runnig your own server, like an Express server, run your functions on a 
scalable, managed, pay-as-you-use service like Amazon Lambda 

- this is a popular choice for small companies 

- you basically just write functions, which get hosted; it's secure, and uses 1 container 
per function


AWS Serverless 

- a tool which helps with creating serverless architectures

- allows us to deploy Amazon Lambda functions easily 

https://aws.amazon.com/serverless/