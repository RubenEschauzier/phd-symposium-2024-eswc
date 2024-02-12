## State of the Art
{:#LiteratureReview}

First, existing approaches for LTQP optimization must be considered. Then, the following sections will discuss existing SPARQL optimization approaches that can be adapted to personalized LTQP optimization.

<!-- due to the wide scope of personalized query optimization, the literature review should consider many fields of related traditional SPARQL optimization literature. The engine can pre-compute statistical information or indexes of often visited documents to speed up both query execution and aid in link prioritisation. Furthermore, the engine might cache (partial) query results that are often visited. Additionally, learned optimization algorithms can assist query planning by using previous experiences to learn to optimize join plans for similar queries. -->

<!-- On the learned optimization side, literature on recommendation systems and learned optimizers for relational and graph databases is highly relevant, as patterns in query execution provide ample learning opportunity for such systems. First, we will consider existing literature on LTQP optimization, then briefly summarize what statistical information in RDF datasets is used for SPARQL query optimization. Finally, we will explore learned optimizers and superficially explain the basics of recommendation systems. -->

<!-- On the one hand, we should use existing knowledge on LTQP or federated query optimization to determine what information should be encoded to aid the optimization approach. Additionally, to determine useful featurization of queries we can use existing literature on learned query optimization for both relational and graph databases. Finally, as client specific optimization is similar to recommendation systems we should investigate current recommendation system approaches that build profiles for each user. Finally, our problem requires online learning of a recommendation system, thus work on continual learning and how to avoid catastrophic forgetting is vital. -->


### Optimizing LTQP
{:#OptimizingLTQP}

LTQP optimization literature aims to both improve the query execution plan and dereference query-relevant documents first. Query relevant documents should be identified by link prioritization algorithms and dereferenced before links that were discovered more recently.

Currently, query planning in LTQP is done using [heuristics](cite:cites hartig2011zero). These heuristics use no prior knowledge and instead uses four rules to determine the evaluation order of operators. First, it evaluates tripe patterns with a seed document first. The exeception to this if the triple pattern has a seed document that represents vocabulary terms. Additionally, it prefers query plans with filtering triple patterns close to the seed triple pattern, finally ensures an order in which preceding triple patterns have atleast one query variable of the proceeding pattern.
[Multiple algorithms](cite:cites hartig2016walking) exist for link priotisation on the Open Linked Data Web, however the approaches do not show a conclusive advantage for any of the algorithms. For structured decentralized environments like Solid [previous work](cite:cites taelman2023link) shows improvement in query execution when using inherent structural information present in structured decentralized environments.

These works assume (almost) no prior knowledge on the data is available, however if our engine queries the same data often, we can use prior knowledge on that data to improve query performance.


### SPARQL Caching Strategies
{:#SPARQLCaching}

In SPARQL, server-side caching stores and reuses repeated computations to reduce the computational complexity of queries [](cite:cites papailiou2015graph, madkour2018worq). 
While entire query results can be cached, most caching approaches instead prefer caching basic graph patterns (BGP) that are seen in many different queries [](cite:cites madkour2018worq). 
These BGPs can replace joins in the query plan and influence join optimization [](cite:cites papailiou2015graph).
To ensure isomorphic BGP are stored only once, [canonical labeling](cite:cites papailiou2015graph) algorithms create unique labels for BGPs, that ensure that all isomorphisms of a BGP get equivalent labels.
Other server-side [caching approaches](cite:cites madkour2018worq) use data summaries to store previously computed optimizations of joins.
These summaries take less space, are more reusable, and still reduce query execution times by an order of magnitude.
<!-- while client-side querying aims to prevent expensive calls to SPARQL endpoints by caching entire query results [](cite:cites zhang2018learning). -->
Client-side caching instead strives to prevent expensive requests to SPARQL endpoints by caching entire query results [](cite:cites zhang2018learning) and [proactive query fetching](cite:cites zhang2016secf).
The performance of such approaches relies heavily on the cache hitrate.
To determine which queries to prefetch, machine learning is used to predict likely subsequent queries, given a current query [](cite:cites zhang2018learning).
These results are then cached, however, if the cache is full the cache entity thats the least likely to be requested has to be removed.
Cache eviction algorithms are numerous, with many different performance profiles. An often used algorithm is the Least Recently Used (LRU), which simply evicts the cache entry that has least recently been requested by the engine.


### Auxillary Data Structures
{:#AuxillaryDataStructures}
In this section we will briefly discuss various datastructures used by query engines to optimize query plans. 
These data structures are often computed in an offline fashion, which makes them infeasable for LTQP. 
However, if we can cache these data structures, and dynamically discover them during LTQP query execution, the same optimization strategies used in traditional querye engines can be translated to LTQP. 
Possible datastructures include:

- **Dataset summaries**, like the [Vocabulary of Interlinked Datasets (VoID)](cite:cites alexander2011describing), describe statistical information of the underlying dataset. This can include the number of triples, distinct subjects or predicates, and the occurences of predicates.
- [**Characteristic sets**](cite:cites neumann2011characteristic) define entities with the same predicate set present in the data. Characteristic sets are used to estimate cardinality of star-shaped joins, which in turn can improve join planning.
- **Approximate Membership Functions (AMFs)** determine whether a dataset can contain answers to a query or not. Examples of AMFs are [Prefix-Partitioned Bloom Filters (PPBFs)](cite:cites aebeloe2019decentralized) and the [extended Semantically Partitioned Bloom Filters (SPBFs)](cite:cites aebeloe2022lothbrok)
- **Indexes** are used to speed-up the look-up of matching triples to triple patterns. Engines calculate different combinations of SPO indexes depending on their implementation. 


<!-- These works often consider the case of an engine running as a SPARQL endpoint over a single dataset, thus these approaches are not directly applicable to LTQP. However, for a personalized engine, that will often query similar data due to client usage patterns, we can compute statistics for often accessed data. Thus,  -->

<!-- - Characteristic sets / hierarchical characteristic sets
- Mention worst-case optimal but mention it doesn't use statistics so not that interesting
- What is a VS tree index? -> gStore: a graph-based SPARQL query engine -->


<!-- - Walking without a map
- Zero knowledge query planning by olaf
- Ruben T paper -->
<!-- ### Recommendation Systems
{:##RecommendationSystems}
Recommendation systems fall into two categories: .. and ... We are interested in recommendation systems that create a profile to describe the user interests. This approach can be translated to instead represent a client's querying behavior. -->

### Learned Optimizers
{:#LearnedOptimizers}
Learned query optimizers in relational databases literature [](cite:cites yu2020reinforcement, marcus2021bao) are currently experiencing a surge in interest.
These optimizing agents are trained using reinforcement learning, which allows them to train in an online setting.
The first step is to convert queries into numeric vectors that contain information useful for query planning. 
Featurization approaches range from [one-hot encoding join predicates](cite:cites marcus2018deep), to applying advanced [graph neural networks](cite:cites yu2020reinforcement) on the query graph.'
The second step is to greedily build a join plan that minimizes the predicted query execution cost or latency. 
To predict query execution latency, [ReJOIN](cite:cites marcus2018deep) uses a feed-forward neural network, while more recent works [](cite:cites yu2020reinforcement, marcus2021bao) use tree-based neural networks to account for the inherent tree structure of join plans.
Finally, the query is executed and the latency is recorded. 
The model is then trained to minimize the difference between predicted latency and actual recorded latency.
While most approaches learn an optimizer from scratch using this methodology, [Bao](cite:cites marcus2021bao) insteads learns to augment traditional query optimizers.
It augments query execution plans by selecting the optimal query hint from a predetermined set of query hints.
This significantly reduces training cost, while still maintaining a signficant improvement over just using a traditional query optimizer.

<!-- Waarom doen we geen merge joins? https://www.google.com/search?client=firefox-b-d&q=SPARQL+optimization+query#fpstate=ive&vld=cid:755dd3ce,vid:TxdnnmgIs5M,st:2356 
Is het omdat we geen sortedness kunnen aannemen?

-->
<!-- State of the art papers to look at:
SOTA:
Look at: https://link.springer.com/article/10.1007/s00778-021-00676-3 
Cool sparql optimization presentation: http://events17.linuxfoundation.org/sites/events/files/slides/SPARQL%20Optimisation%20101%20Tutorial.pdf 
Look into data profiling

- Continual representation learning  / current approaches of encoding SPARQL information

- LTQP over decentralized structural environments

- Federated query planning approaches

- Learned SQL optimizers over single datasource

- recommendation system sota / cold start problem 

- Continual learning sota

- Work on browsing sessions

- Multiple query optimization -->