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

## Reviews
**System Design Engineer: Dennis Arnold**

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
