# System Design Capstone
A complete back-end system to support an e-commerce retail client and sustain up to 10,000 requests per second.

### Inherited Codebase
This project inherited an existed front-end client, built on React, with the ultimate task of supporting millions of relational records, serving data with minimal response times, and supporting client traffic up to 10,000 requests per second.

### Breakdown
The e-commerce client is broken down into three sections: product overview, product questions and answers, and ratings and reviews. Each engineer designed and deployed a database, servers, and load balancer (linked above, with the front-end client) for each section.

## Product Overview
**System Design Engineer: Jim Burch**

[Screenshot and/or gif]



## Q&A
**System Design Engineer: Emma Knor**

Scaled service to handle up to 10,000 RPS.
![Screen Shot 2021-03-27 at 12 50 56 PM](https://user-images.githubusercontent.com/73598239/114930646-7e354980-9df2-11eb-9d97-8c6e6b6cc642.png)


[Screenshot and/or gif]
DESCRIPTION HERE

## Reviews
**System Design Engineer: Dennis Arnold**

[Screenshot and/or gif]
DESCRIPTION HERE

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
