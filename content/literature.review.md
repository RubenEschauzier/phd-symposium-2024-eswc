## State of the Art
{:#LiteratureReview}

First, existing approaches for LTQP optimization must be considered. Then, the following sections will discuss existing SPARQL optimization approaches that can be adapted for personalized LTQP optimization.

### Optimizing LTQP
{:#OptimizingLTQP}

The literature on LTQP optimization aims to improve both the execution plan of queries and the prioritization of query-relevant documents. Identification of query-relevant documents is crucial and typically relies on link prioritization algorithms, ensuring that these documents are accessed before others. 
Presently, LTQP query planning relies on [heuristics](cite:cites hartig2011zero).
These heuristics, which use no prior knowledge, employ four rules to establish the evaluation order of operators. 
Firstly, they prioritize triple patterns with a designated seed document, except when the seed document represents vocabulary terms. 
Moreover, they favor query plans featuring filtering triple patterns in proximity to the seed triple pattern.
Finally, they create an order where preceding triple patterns contain at least one query variable of the subsequent pattern.

While [multiple algorithms](cite:cites hartig2016walking) for link prioritization exist for the Open Linked Data Web, none show a definitive advantage over others.
However, in structured decentralized environments like Solid, previous [work](cite:cites taelman2023link), demonstrates enhanced query execution when leveraging structural information inherent to such environments.

These studies usually assume limited prior data knowledge. 
However, when our engine frequently queries the same dataset, leveraging prior knowledge can greatly boost query performance.

### SPARQL Caching Strategies
{:#SPARQLCaching}

In SPARQL, server-side caching optimizes query performance by storing and reusing computations [](cite:cites papailiou2015graph, madkour2018worq). 
While caching entire query results is possible, most strategies focus on frequently encountered basic graph patterns (BGPs) [](cite:cites madkour2018worq). 
These BGPs can substitute joins in query plans and influence join optimization [](cite:cites papailiou2015graph). 
Canonical labeling algorithms assign distinct labels to isomorphic BGPs, ensuring that all isomorphic BGPs receive equivalent labels [](cite:cites papailiou2015graph).
Other server-side [caching approaches](cite:cites madkour2018worq) utilize data summaries to reuse previously computed join optimizations. 
Client-side caching aims to minimize requests to SPARQL endpoints by caching complete query results [](cite:cites zhang2018learning) and implementing [proactive query fetching](cite:cites zhang2016secf).
The efficacy of such strategies heavily depends on the cache hit rate.
To decide which queries to prefetch, machine learning techniques predict probable subsequent queries based on the current query[](cite:cites zhang2018learning).
When the cache reaches capacity, cache eviction algorithms, such as Least Recently Used (LRU), remove the least recently requested entry.


### Auxillary Data Structures
{:#AuxillaryDataStructures}

In this section, we'll briefly examine data structures used by query engines to optimize query plans. 
While these are typically precomputed offline, making them impractical for LTQP, caching and dynamically discovering them during LTQP query execution could allow the LTQP engine to use traditional SPARQL optimization strategies.
Potential structures include:

- **Dataset summaries**, such as the [Vocabulary of Interlinked Datasets (VoID)](cite:cites alexander2011describing), describe statistical information of the underlying dataset. This can include the number of triples, distinct subjects or predicates, and the occurrences of predicates.
- [**Characteristic sets**](cite:cites neumann2011characteristic), which define entities sharing the same predicate set present in the data. Characteristic sets are instrumental in estimating the cardinality of star-shaped joins, thereby enhancing join planning.
- **Approximate Membership Functions (AMFs)**, which determine whether a dataset can potentially contain answers to a query or not. Examples of AMFs are [Prefix-Partitioned Bloom Filters (PPBFs)](cite:cites aebeloe2019decentralized) and the [extended Semantically Partitioned Bloom Filters (SPBFs)](cite:cites aebeloe2022lothbrok)
- **Indexes**, which are utilized to accelerate the lookup of matching triples to triple patterns. Engines calculate different combinations of SPO indexes depending on their implementation. 


### Learned Optimizers
{:#LearnedOptimizers}

Recent literature on learned query optimizers in relational databases [](cite:cites yu2020reinforcement, marcus2021bao) is gaining traction, utilizing reinforcement learning for online training. 
Queries are first transformed into numeric vectors containing information crucial for query planning, with various featurization methods, such as one-hot encoding join predicates [](cite:cites marcus2018deep) or utilizing advanced graph neural networks on the query graph [](cite:cites yu2020reinforcement). 
The next step involves greedily constructing a join plan to minimize predicted execution cost or latency. 
To predict latency, [ReJOIN](cite:cites marcus2018deep) employs a feed-forward neural network, while newer approaches use tree-based neural networks to handle the tree structure of join plans [](cite:cites yu2020reinforcement, marcus2021bao). 
The model is trained to minimize the difference between predicted and actual query latency or cost. 
While most approaches train optimizers from scratch, [Bao](cite:cites marcus2021bao) augments traditional optimizers by learning to select optimal query hints from a predefined set, reducing training costs significantly while still improving over traditional optimizers.