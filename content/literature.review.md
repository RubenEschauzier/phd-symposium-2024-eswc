## State of the Art
{:#LiteratureReview}

First, existing approaches for LTQP optimization must be considered. Then, the following sections will discuss existing SPARQL optimization approaches that can be adapted for personalized LTQP optimization.

<!-- due to the wide scope of personalized query optimization, the literature review should consider many fields of related traditional SPARQL optimization literature. The engine can pre-compute statistical information or indexes of often visited documents to speed up both query execution and aid in link prioritisation. Furthermore, the engine might cache (partial) query results that are often visited. Additionally, learned optimization algorithms can assist query planning by using previous experiences to learn to optimize join plans for similar queries. -->

<!-- On the learned optimization side, literature on recommendation systems and learned optimizers for relational and graph databases is highly relevant, as patterns in query execution provide ample learning opportunity for such systems. First, we will consider existing literature on LTQP optimization, then briefly summarize what statistical information in RDF datasets is used for SPARQL query optimization. Finally, we will explore learned optimizers and superficially explain the basics of recommendation systems. -->

<!-- On the one hand, we should use existing knowledge on LTQP or federated query optimization to determine what information should be encoded to aid the optimization approach. Additionally, to determine useful featurization of queries we can use existing literature on learned query optimization for both relational and graph databases. Finally, as client specific optimization is similar to recommendation systems we should investigate current recommendation system approaches that build profiles for each user. Finally, our problem requires online learning of a recommendation system, thus work on continual learning and how to avoid catastrophic forgetting is vital. -->


### Optimizing LTQP
{:#OptimizingLTQP}

The literature on LTQP optimization aims to improve both the execution plan of queries and prioritization of query-relevant documents. Identification of query-relevant documents is crucial and typically relies on link prioritization algorithms, ensuring that these documents are accessed before others. 
Presently, LTQP query planning relies on [heuristics](cite:cites hartig2011zero).
These heuristics, which use no prior knowledge, employ four rules to establish the evaluation order of operators. 
Firstly, they prioritize triple patterns with a designated seed document, except when the seed document represents vocabulary terms. 
Moreover, they favor query plans featuring filtering triple patterns in proximity to the seed triple pattern.
Finally, they create an order where preceding triple patterns contain at least one query variable of the subsequent pattern.

While [multiple algorithms](cite:cites hartig2016walking) for link prioritization exist for the Open Linked Data Web, none show a definitive advantage over others.
However, in structured decentralized environments like Solid, previous [work](cite:cites taelman2023link), demonstrates enhanced query execution when leveraging structural information inherent to such environments.

These studies typically operate under the assumption of limited prior knowledge about the data. 
However, in cases where our engine frequently queries the same dataset, harnessing prior knowledge can significantly enhance query performance.

### SPARQL Caching Strategies
{:#SPARQLCaching}

In SPARQL, server-side caching efficiently stores and reuses repeated computations to mitigate the computational complexity of queries [](cite:cites papailiou2015graph, madkour2018worq). 
While caching entire query results is an option, most strategies focus on caching basic graph patterns (BGPs) that are frequently encountered across multiple queries [](cite:cites madkour2018worq). 
These BGPs can effectively substitute joins in query plans and influence join optimization (cite:cites papailiou2015graph). 
To ensure uniqueness, canonical labeling algorithms assign distinct labels to isomorphic BGPs, ensuring that all isomorphic patterns receive equivalent labels (cite:cites papailiou2015graph).
Other server-side [caching approaches](cite:cites madkour2018worq) utilize data summaries to retain previously computed join optimizations. These summaries occupy less space, offer greater reusability, and still significantly reduce query execution times [caching approaches](cite:cites madkour2018worq). 
Client-side caching, on the other hand, aims to circumvent expensive requests to SPARQL endpoints by caching complete query results [](cite:cites zhang2018learning) and implementing [proactive query fetching](cite:cites zhang2016secf).
The efficacy of such strategies heavily depends on the cache hit rate.
To decide which queries to prefetch, machine learning techniques are employed to predict probable subsequent queries based on the current query[](cite:cites zhang2018learning).
If the cache reaches capacity, the least likely cache entity to be requested must be evicted.
Cache eviction algorithms vary widely, each offering different performance profiles.
One commonly used algorithm is Least Recently Used (LRU), which simply removes the cache entry that has been least recently requested by the engine.



### Auxillary Data Structures
{:#AuxillaryDataStructures}
In this section, we will briefly explore various data structures utilized by query engines to optimize query plans. 
These data structures are typically computed offline, rendering them impractical for LTQP.
However, if these data structures can be cached and dynamically discovered during LTQP query execution, the same optimization strategies employed in traditional query engines can be adapted to LTQP.
Potential data structures include:


- **Dataset summaries**, such as the [Vocabulary of Interlinked Datasets (VoID)](cite:cites alexander2011describing), describe statistical information of the underlying dataset. This can include the number of triples, distinct subjects or predicates, and the occurences of predicates.
- [**Characteristic sets**](cite:cites neumann2011characteristic), which define entities sharing the same predicate set present in the data. Characteristic sets are instrumental in estimating the cardinality of star-shaped joins, thereby enhancing join planning.
- **Approximate Membership Functions (AMFs)**, which determine whether a dataset can potentially contain answers to a query or not. Examples of AMFs are [Prefix-Partitioned Bloom Filters (PPBFs)](cite:cites aebeloe2019decentralized) and the [extended Semantically Partitioned Bloom Filters (SPBFs)](cite:cites aebeloe2022lothbrok)
- **Indexes** utilized to accelerate the lookup of matching triples to triple patterns. Engines calculate different combinations of SPO indexes depending on their implementation. 


### Learned Optimizers
{:#LearnedOptimizers}
Learned query optimizers in relational databases literature [](cite:cites yu2020reinforcement, marcus2021bao) are currently witnessing a surge in interest.
These optimizing agents are trained using reinforcement learning, enabling them to train in an online setting.
The first step involves converting queries into numeric vectors containing information crucial for query planning. 
Featurization approaches span from one-hot encoding join predicates (Marcus et al., 2018) to leveraging advanced graph neural networks on the query graph (cite:cites yu2020reinforcement)
The second step entails greedily constructing a join plan that minimizes the predicted query execution cost or latency. 
To predict query execution latency, [ReJOIN](cite:cites marcus2018deep) employs a feed-forward neural network, while more recent works [](cite:cites yu2020reinforcement, marcus2021bao) utilize tree-based neural networks to accommodate the inherent tree structure of join plans.
Finally, the query is executed, and the latency is recorded.
The model is then trained to minimize the disparity between predicted latency and actual recorded latency.
While most approaches learn an optimizer from scratch using this methodology, [Bao](cite:cites marcus2021bao) instead learns to augment traditional query optimizers. 
It enhances query execution plans by selecting the optimal query hint from a predetermined set of query hints. 
This approach significantly reduces training cost while still yielding a substantial improvement over merely employing a traditional query optimizer.