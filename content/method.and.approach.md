## Research Methodology and Approach
{:#method}
The work in this thesis is divided into three packages. The first aims to answer **RQ I**, the second provides an answer to **RQ II & III**, and finally the third will investigate **RQ IV**.

### Identification and Simulation of Client-Specific Query Usage Patterns
{:#UsagePatternMethod}

Evaluating the effectiveness of personalized query optimization algorithms requires a benchmark simulating real-world query patterns.
To identify these patterns, the first step is a literature review on sub-communities in social media networks and the use of linked data in existing applications.
This literature review will be used in the creation of a theoretical framework outlining various client query usage patterns in social media and linked data.

An existing benchmark,  [SolidBench](cite:cites taelman2023link), simulates a social network application's data that is fragmented to represent Solid data vaults.
Extending SolidBench using the theoretical framework of client query usage patterns allows for simulated client-specific query sequences, representing real-world sub-communities and access patterns.
The degree of fragmentation and separation between communities and the probability of within-community queries will be an adjustable parameter to enable the analysis of varying degrees of client-specific query patterns.

### Caching in the Context of Link Traversal-based Query Processing
{:#CachingMethod}

While caching entire query results is straightforward, caching intermediate results in LTQP is complicated due to the non-stationarity of the queried data, which changes depending on the query predicates.
Intermediate result caches must be aware of the underlying documents that produced these results.
In doing so, the engine can identify over what data these intermediate results are valid and what data is not included in the results.
To answer **RQ II** an adaptive query planner that can adaptively change its execution plan to include intermediate results that are valid for the currently dereferenced documents is needed.
This query planner will need to consider three cases. 
First, the case where the intermediate results contain results that are produced using undiscovered data. 
In this case, careful pruning of cache elements is required.
Second, the cache can contain less data than is discovered during query execution.
The query planner should in that case first use all valid intermediate results to quickly produce answers, however, the additional data should be included in the query execution as well to ensure result completeness.
Finally, these two cases can occur at the same time which will require a combination of the solutions of the previous cases.
The proposed query planner must be adaptive, as the validity of the cache can change as more documents are discovered.

For **RQ III**, an investigation into the effect of the data structures described in [](#AuxillaryDataStructures) is required.
Data structures like approximate membership functions, dataset summaries, and characteristic sets can be used to determine whether a document can produce answers to a given query.
Documents that will never return results to the query can be pruned, reducing the queried data size and improving query execution times.
Additionally, dataset summaries and characteristic sets can be used to improve cardinality estimation as they are discovered.
This allows the engine to improve its query plan resulting in reduced computational complexity of the query.
Finally, the possible upside of indexes is clear.
Currently, these indexes are computed as the engine dereferences a document, which is computationally intensive.
If the engine can reuse indexes from previous queries, we bypass the need to re-compute them.

The fundamental risks of all caching approaches are the overhead of maintaining the cache and the possibility of cache invalidation.


the cost of determining a cache hit, and handling the cache size.
If searching the cache for relevant entries is too computationally intensive and the cache hit rate is low, the engine will spend more time searching the cache than it saves using cache entries.
To account for this risk, this thesis will first investigate caching approaches that require the least complex cache keys.
These approaches are either caching entire query result sets or caching data on a per-document basis.
Document URLs are unique, thus they can be used as cache keys without preprocessing.
Only after successfully applying the straightforward caching approaches will more complex tasks be considered.
To tackle the cache size issue, we will leverage existing literature on cache eviction strategies. 
We plan to investigate both the impact of cache size and eviction strategy by testing multiple configurations and comparing their respective effects on cache performance

### Learned Query Optimization in Link Traversal-based Query Processing
{:#LearnedOptimizationMethod}
To answer **RQ IV**, personalized query engines need learned query optimization algorithms that work in an online setting since LTQP engines do not know what data they will query in advance.
As such, any offline training algorithm that requires millions of training examples is by definition infeasible.
My previous work in [SPARQL join order optimization](cite:cites eschauzier2023reinforcement) shows that while reinforcement learning-based join order optimization is promising, it is computationally expensive to train an optimizer from scratch.
Instead, [learned query optimization hint approaches](cite:cites marcus2021bao, woltmann2023fastgres) train models that give hints to existing optimizers, like what join operator to use.
These approaches require significantly less training time and are thus more suitable for online learning.
To answer **RQ IV**, these relational learned query optimization hints will be adapted for use in LTQP.



