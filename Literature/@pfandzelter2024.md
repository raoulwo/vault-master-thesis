---
author: Pfandzelter T., Bermbach D.
year: 2024
tags:
  - serverless
  - orbital-edge-computing
pdf_link: "[[Komet__A_Serverless_Platform_for_Low-Earth_Orbit_Edge_Services.pdf|Open PDF]]"
---
# Komet: A Serverless Platform for Low-Earth Orbit Edge Services

## TLDR

%% One-sentence summary of the core/claim contribution. %%

*Komet*, a serverless platform for low-Earth orbit edge computing is introduced.

## Thoughts

%% Thoughts and questions about the paper. %%

- Simple service scheduling heuristics are designed and evaluated -> Are more sophisticated algorithms (learning-based) appropriate?
- Is decoupling of state from compute worth it for ease of migration? What are the downsides?
- A centralized scheduler is proposed -> What if we have decentralized schedulers (better scalability, however more complex), what would be the benefits however?
- Scheduling heuristics: Latency vs. number of migrations
- Komet leverages satellite trajectory calculations for scheduling migrations -> What if we could utilize edge AI models for prediction rather than calculations (I don't know if less compute-intensive)
## Related Work

%% Existing state-of-the-art research. %%

A unique challenge of LEO edge computing is *service orchestration*. LEO satellites **must** move at speeds in excess of 27.000 km/h in relation to Earth, which results in frequent (4-5 minutes) client handovers.

The solution to this mobility is *virtual stationarity*, where services transparently remain in client proximity by frequent service migration thus offsetting the satellites' movements.

Kubernetes mandates downtime for frequent migrations, instead serverless computing abstractions are used to achieve virtual stationarity without downtime.

## Contributions

%% Research gap and novel contributions. %%

The following contributions are made:

- *Komet* is introduced: A serverless platform for the LEO edge that combines FaaS compute platform with a distributed data management layer and automatically migrates services to provide virtual stationarity
- Heuristics for scheduling LEO edge services are introduced
- A proof-of-concept prototype is implemented and evaluated on *Celestial* testbeds
- Trade-offs between migration frequency and service level in scheduling LEO edge compute services are investigated

Future work:

- Moving from a centralized to a decentralized scheduler for service migrations

Case studies:

- Internet of Things
- CDN

Limitations and Future Work:

- Serverless can't easily support applications with continuous inputs/outputs such as live audio transcoding or video conferencing (long-running computations) -> Continuous stream abstraction in serverless
- Replicated data consistency: Read that section again, don't understand it fully
- Satellite server selection: Proposed heuristics already have shown improvement, however finding optimal schedule requires far more research
- Scheduler location: Authors believe that existence of centralized scheduler is good enough, however future research for different scheduler architectures may be possible -> distributed per-satellite autonomous schedulers + intelligent scheduling algorithms?
- Failover mechanisms in case of on-board server failures due to radiation must be in place to offer continuous coverage

## Methodology

%% Architecture of the solution. %%

Komet follows the intuition that *decoupling* state from compute enables transparent live migration of services. Additionally, only state (not the entire software stack) has to be replicated for the application to appear as monolithic rather than a distributed application.

Migration of stateless serverless functions (since already uploaded to all nodes) encompasses only the data replication (i think).

The migration requires scheduling -> A centralized, per-application scheduler (e.g. cloud-based) is proposed.

The scheduler is responsible for:

- Proactively deploying functions and data replicas to satellite servers
- Informing clients when they should connect to a new satellite server
- Removing functions and data replicas from servers when they are no longer used

Heuristics for scheduling: Leverage calculating the trajectory of satellites, then based on distance to client, the destination node for migration is determined (only if distance improves by threshold)

## Implementation

%% Actual implementation of the solution. %%

*tinyFaaS* + *FReD*

## Evaluation

%% The evaluation of the solution. %%

Hand-off (replication occurs roughly 20.8s before hand-off) leads to consistent request-response times without downtime.

Replication time scales with size of data in data store: Komet's replication delay is comparatively lower than using containers, while not leading to service downtime.

Comparison between MinMax scheduling algorithm with heuristic based on different thresholds:

- RTT stays similar between all strategies
- Time between service migrations significantly increased for 25% heuristic, reducing the overall number of migrations

---

## Links

%% Links to related literature. %%