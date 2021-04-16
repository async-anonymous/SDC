# System Design Capstone
A complete back-end system to support an e-commerce retail client and sustain up to 10,000 requests per second.

### Inherited Codebase
This project inherited an existed front-end client, built on React, with the ultimate task of supporting millions of relational records, serving data with minimal response times, and supporting client traffic up to 10,000 requests per second.

### Breakdown
The e-commerce client is broken down into three sections: product overview, product questions and answers, and ratings and reviews. Each engineer designed and deployed a database, servers, and load balancer (linked above, with the front-end client) for each section.

## Product Overview
**System Design Engineer: Jim Burch**

### Serving Millions of Records
The client application needed to maintain transactional and inventory persistence, which called for a relational database such as PostgreSQL. The challenge with millions of records and complex joins was speed.

Key indexing and aggregate methods (something that puts PostgreSQL above other RDMS like MySQL) brought search queries from a snail's pace of 25 seconds all the way down to 5ms when searching the last 10% of the database records.

### Finding the Bottleneck
The pursuit towards 10,000 requests per second hit a bottleneck once the system was deployed to AWS. Even with 10 EC2 instances and an NGINX load balancer, the system was stalling at 1750 clients per second.

![](/screenshots/1750test.png)
![](/screenshots/bottleneck.png)

Process of elimination was the best path to a solution, starting with the database. pgBench found that PostgreSQL didn't flinch at 1750 requests and a CLI tool called HTOP showed that each server was only using roughly 15% of its CPU power during the same test. That left the load balancer.

Testing the native AWS load balancer exposed the bottleneck in NGINX (or most likely the hardware limitations of the EC2 instance it was hosted on) and the system hit the 10,000 rps benchmark.

## Q&A
**System Design Engineer: Emma Knor**

### Optimization
In order to ensure transactional integrity and minimize duplicate records, the Q&A service is supported with a relational database. The initial implementations employed complex left outer joins to access and provide neccessary data for the client. However, query times proved to be far to long considering the client demands were for query times come out in under 5ms. All queries were reduced to under 1ms utilizing column indexing to provide the database with a reference point for rapid lookup as the query plan indicated filtering occupied the greatest amount of time. Additionaly aggregate tables optimized query times by providing a way for related data to be stored in one table while maintaining a single source of truth.

### Stress Testing
The Q&A service was stress tested with k6 tests. The predominant metrics were RPS, latency, and error rate. Tests were performed sequentially based on RPS, incrementing from 10, 100, 1,000, and then finally 10,000 RPS. In order to locate the bottleneck within this service pgBench tests proved the database to be capable of more than 10,000 RPS, which isolated the limitation to the servers and load balancers. From there an additional iteration was performed with an AWS load balancer which enabled the service to handle the goal of 10,000 RPS.

1,000 RPS
 [Screen Shot 2021-03-27 at 12.50.56 PM.pdf](https://github.com/async-anonymous/SDC/files/6321864/Screen.Shot.2021-03-27.at.12.50.56.PM.pdf)

10,000 RPS
![Screen Shot 2021-04-15 at 6 31 34 PM](https://user-images.githubusercontent.com/73598239/114957460-50fe9080-9e1e-11eb-959d-35856d83c454.png)

## Reviews
**System Design Engineer: Dennis Arnold**

### The Data
For the purposes of consistency across all databases and not repeating values that were used in multiple places, PostgreSQL was chosen for this database. There were over 10 million individual records that needed to be inserted into multiple tables of the database and some data was incomplete. For the purposes of meeting the deadline, the records that were incomplete were individually deleted and re-inserted with appropriate values into the database after creation. 

Using multiple tables with millions of records led to some queries being quite complex, including multiple joins and aggregate methods. This caused some queries to take much longer than was needed to meet our goal of 10k rps, but after some refactoring and proper indexing query times were reduced to less than 1ms even for complex queries.

### Scaling
After getting the database to query with appropriate speed, it was determined that the server was going to be the piece that was no longer able to keep up with requests. Since we wanted a single source of truth, we decoupled our databases from our server instances and began to horizontally scale server instances. Finding diminishing returns with each instance after 10 running servers, we implemented an Nginx load balancer for each database and set of server instances. This greatly increased performance, but after testing both our servers and our databases, we determined it was still the load-balancer holding us back. After testing with the AWS load balancer, we were able to reach the required 10k rps.


## Tech Stack
- PostgreSQL
- AWS (EC2, Load Balancer)
- NGINX
- k6 / Grafana / Influx
- Loader.io

## Contributors
- [Dennis Arnold](https://github.com/DennisJArnold)
- [Emma Knor](https://github.com/emmaknor)
- [Jim Burch](https://github.com/JimBurch)
