# Comparative Study: Graph Model vs. Relational Model on a Public Transport Network

<img width="1014" height="699" alt="image" src="https://github.com/user-attachments/assets/0dda272b-51e3-4953-a7e9-7d6db74d2814" />

Graph visualization of Swiss Trains

---

## Project Overview

Transportation networks inherently consist of highly connected data, making them a natural fit for graph-based representations. This project explores the structural and computational differences between the graph database model (Neo4j) and the relational database model (PostgreSQL) when handling complex network topologies.

Specifically, the study evaluates the impact of query syntax and engine semantics on expressiveness, execution strategies, and memory consumption. We analyze classic graph queries, new language features, and pure relational recursive approaches.

## Methodology and Data Modeling

The underlying dataset is derived from the General Transit Feed Specification (GTFS), a standard format for public transportation schedules. To maintain computational feasibility and avoid intractability during complex recursive queries, the experiments were conducted on a carefully isolated experimental subgraph.

* **Graph Implementation (Neo4j):** The network was modeled by converting tabular events into a pure graph topology. Stations and trips are represented as distinct nodes, while the actual flow of transport is materialized through explicit, directed, and weighted edges (the `ST_NEXT` relationship).


* **Relational Implementation (PostgreSQL):** A robust schema was created to mirror the network's structure, utilizing junction tables that abstract tabular data into source-to-target edge representations, enabling algorithmic pathfinding via SQL.



## Query Typologies Analyzed

The core of the project involves benchmarking the systems against three main categories of graph problems:

1. **Sequential Edge Filtering:** Testing the engines' ability to filter paths based on strictly increasing edge properties (e.g., travel durations). This compares standard query language paradigms (like Cypher 5's `NOT EXISTS` clauses) against newer functional reductions (`allReduce` in Cypher 25) and standard SQL joins.


2. **Variable-Length Traversals:** Evaluating Quantified Graph Patterns (QGP). This section tests how well the different database engines handle structural pattern matching over unknown path lengths, which inherently risks triggering a combinatorial explosion.


3. **Algorithmic Graph Computing:** A stress test focusing on shortest path calculations. We constructed artificially elongated "virtual paths" to push declarative query engines to their limits. This allowed us to contrast standard declarative path enumeration against specialized, in-memory graph algorithms (Neo4j Graph Data Science leveraging Breadth-First Search and Dijkstra) for both weighted and unweighted scenarios.



## General Findings

The comparative analysis highlights significant differences depending on the architectural approach used.

We observed that standard declarative engines, whether utilizing graph pattern matching or recursive SQL Common Table Expressions, rely heavily on path enumeration. In scenarios involving deep traversals or complex weighted calculations, this enumeration can lead to massive intermediate data generation and eventual memory exhaustion. Conversely, specialized algorithmic frameworks bypass this limitation by using incremental relaxation strategies, maintaining stability regardless of path depth. Relational databases, on the other hand, proved highly efficient for shallow, structured join operations but struggled with the combinatorial nature of variable-length graph queries.

**For a detailed technical breakdown, including execution plans (EXPLAIN/PROFILE), exact execution times, memory allocation metrics, between Cypher 5, Cypher 25, and PostgreSQL --> Please refer to the full PDF report included in this repository.**


**Authors:** Rabah ACHOUR & Mohand Arezki BRANECI 
