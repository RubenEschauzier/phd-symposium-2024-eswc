## Research Methodology and Approach
{:#method}
The work in this thesis is divided into three packages. The first aims to answer **RQ I**, the second provides an answer to **RQ II & III**, and finally the third will investigate **RQ IV & V**.

### Identification and Simulation of Client-Specific Query Usage Patterns
{:#UsagePatternMethod}

Evaluating the effectiveness of personalized query optimization algorithms requires a benchmark simulating real-world query patterns.
To identify these patterns, the first step is a literature review on sub-communities in social media networks and the use of linked data in existing applications.
This literature review will be used in the creation of a theoretical framework outlining various client query usage patterns in social media and linked data.

An existing benchmark,  [SolidBench](cite:cites taelman2023link), simulates a social network application's data that is fragmented to represent Solid data vaults.
Extending SolidBench using the theoretical framework of client query usage patterns allows for simulated client-specific query sequences, representing real-world sub-communities and access patterns.
The degree of fragmentation and separation between communities and the probability of within-community queries will be an adjustable parameter to enable the analysis of how varying degrees of client-specific query patterns influence personalized query optimization performance.

### Caching in the Context of Link Traversal-based Query Processing
{:#CachingMethod}

While caching entire query results is straightforward, caching intermediate results in LTQP is complicated as the queried data changes depending on the query predicates.
Intermediate result caches must be aware of the underlying documents that produced these results to allow the engine to identify over what data these intermediate results are valid and what data is not included in the cached results.
To answer **RQ II**, an adaptive query planner that can adaptively change its execution plan to include intermediate results that are valid for the currently dereferenced documents is needed.
Using cached intermediate results, we can reuse computation, quickly check document cache validity through ETags, and produce first results faster.
This query planner will need to consider three cases. 
The first case is where the intermediate results contain results produced using undiscovered data. 
In this case, careful pruning of cache elements is required.
Second, the cache can contain less data than discovered during query execution.
In this case, the query planner should first use all valid intermediate results to quickly produce answers before including the additional data in the query execution to ensure result completeness.
Finally, these two cases can occur simultaneously, which will require a combination of the solutions of the previous cases.
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
If searching the cache for relevant entries is too computationally intensive and the cache hit rate is low, the engine will spend more time searching the cache than it saves using cache entries.
To account for this risk, this thesis will first investigate caching approaches requiring the least complex cache keys, like document-based caching, or query result caching.
After successfully applying the straightforward caching approaches complex tasks will be considered.
For cache invalidation, we must account for the possibly rapidly changing data landscape in social media applications. 
However, even when many _new_ posts are added, cached information for old and unchanged content remains valid and can improve query execution performance.
Furthermore, even if the query is primarily over the subset of data that rapidly changes, caching the static content in social-media applications can inform the engine of their (ir)relevancy, reducing the queried data size and improving performance.
To determine whether a cached entity is valid, we can use the ETag header or introduce data vault server-side data structures that indicate the last change to a resource.

### Learned Query Optimization in Link Traversal-based Query Processing
{:#LearnedOptimizationMethod}
To answer **RQ IV**, personalized query engines need learned query optimization algorithms that work in an online setting since LTQP engines do not know what data they will query in advance.
As such, any offline training algorithm that requires millions of training examples is infeasible.
My previous work in [SPARQL join order optimization](cite:cites eschauzier2023reinforcement) shows that while reinforcement learning-based join order optimization is promising, it is computationally expensive to train an optimizer from scratch.
Instead, [learned query optimization hint approaches](cite:cites marcus2021bao, woltmann2023fastgres) train models that give hints to existing optimizers, like what join operator to use.
These approaches require significantly less training time and are thus more suitable for online learning.
To answer **RQ IV & V**, relational learned query optimization hints will be adapted for use in LTQP.
