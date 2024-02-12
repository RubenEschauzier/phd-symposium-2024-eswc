<!-- ## Misc Notes
{:#MiscNotes}


Topic of paper:
Personalized Query Engine Optimization for Link Traversal-based Query Processing over Structured Decentralized Environments

Link Traversal-based Query Optimization executes queries by traversing links found in previously dereferenced data sources. ... This means that a significant portion of the query execution computional burden is placed on the client, not server. Client-side query optimization over unknown sources is difficult due to the absence of extensive statistical information on the queried data, and the possibly diverse types of data sources discovered. Additionally, the amount of linked data on the internet is possibly infinite (cite olaf paper here). However, when we look at decentralized environments that are closed and have information on the structure of the environment available beforehand we can use this for query optimization. Solid is such a decentralized environment, with certain predicates denoting the way data is stored and can be traversed [](cite:cites taelman2023link)(insert more explanation here).

Furthermore, on a user level-basis some patterns in query behavior may be observed [cite:cites kulshrestha2015characterizing]. Data sources can be more likely to be relevant for any query of the user based on previous engine usage. This idea is similar to content-based recommendation [](cite:cites davidson2010youtube, chen2007content, kulkarni2020context, ko2022survey), which uses (learned) profile of the user to find videos that the user is likely interested in. Furthermore, the engine can use previous query executions to learn what query plan is optimal for different types of queries the user often executes.



(Data aggregators are another example of client that observe patterns in query behavior. These clients will often aggregate the same type of data during its lifetime. Due to the stability in data queried, aggregators are a prime example of user that can benefit optimizers based on a user profiles. (needs paper)) Not sure about this one

Introduction general outline:


State of the art papers to look at:
SOTA:

Personalized optimization strategies is a multi-diciplinary problem. On the one hand, we should use existing knowledge on LTQP or federated query optimization to determine what information should be encoded to aid the optimization approach. Additionally, to determine useful featurization of queries we can use existing literature on learned query optimization for both relational and graph databases. Finally, as client specific optimization is similar to recommendation systems we should investigate current recommendation system approaches that build profiles for each user. Finally, our problem requires online learning of a recommendation system, thus work on continual learning and how to avoid catastrophic forgetting is vital.
- Continual representation learning  / current approaches of encoding SPARQL information

- LTQP over decentralized structural environments

- Federated query planning approaches

- Learned SQL optimizers over single datasource

- recommendation system sota / cold start problem 

- Continual learning sota


(For preventing catastrophic forgetting: https://proceedings.neurips.cc/paper/2021/file/54ee290e80589a2a1225c338a71839f5-Paper.pdf, among others import to find lots of papers)

Problem statement / contributions

RQ1: Can personalized query engine optimizers improve LTQP performance in structured decentralized environments?
    
- RQ1.1 What measures do we need to compare algorithmic efficiency of LTQP approaches?

- RQ1.2 How can we represent prior knowledge on queried data sources obtained during previous query executions?

- RQ1.3 Is it possible to determine expected relevance of documents during LTQP based on prior knowledge representations of queried data?

- RQ1.4 Can prior knowledge representations improve initial query planning during LTQP?
    



Research methodology

RQ 1.1: To assess the performance of LTQP engines, multiple metrics exist. The easiest would be to look at total execution time. However, LTQP is a streaming query approach and the speed at which the first $k$ results arrive is an important aspect of LTQP performance. If the engine quickly produces results, these can already be shown and used by the client. Natural metrics for a streaming engine are time for the first answer and throughput. Furthermore, other available performance indicators are the [diefficiency at $k$ and at $t$](cite:cites acosta2017diefficiency), which captures the answer concentration for the first $k$ results or during time period $t$ respectively. 
However, these metrics measure the performance of the engine as a whole. Comparing specific parts of the query execution across different engines is challenging, as one engine can have better query planning, while the other is better at selecting relevant links to traverse to first. 

Comparisons of join plans are straightforward, as this is primarily reliant on producing the least amount of intermediate results. However, evaluating the performance of the entire query planning is challenging, as the join algorithms used or other optimization strategies are not easily quantified. To isolate the query plan optimziation performance from traversal order performance differences the compared engines should use the same FiFo traversal order.

To compare link traversal performance, I propose a family of metrics that compares the optimal traversal path to visit all documents that contribute to the query results to the traversal path taken by the engine. The engine's traversal path is only dependant on the link traversal strategy, while the optimal traversal path is the same irregardless of query engines used. Thus, such a metric will allow for a deterministic comparison of algorithmic efficiency of link traversal strategies between engines. Furthermore, such a metric will allow insight into how close to optimal our current link traversal algorithms are.

RQ 1.2: Representing prior knowledge obtained during query execution in a way that the engine can use, while being cheap to compute and store is the key challenge of my PhD.
    

Challenges:
 - Finding data that incorporates actual user specific usage. As these approaches require query sets for each user. 
 - Possible solutions: Find social science research on how people use the internet / specific application. Translate this into sparql queries with slight variations per user within a usage group. Create synthetic dataset with specific user profiles [cite:cites kulshrestha2015characterizing]. Make each user a mix of some user profiles to create query sets. Advantage, we can using a parameter influence how different users are to each other and make bigger analysis this way.
 - Determining similarity between datasources before we have queried them. There is very little metadata on these sources. 
 - Possible solutions: String similarity, storing user specific previous executions. Creating context-based vectors to represent datasources as we go. Then compare these vectors to vector in query. Question: How can be encode query rdf terms properly.
 - Possiblity of datasource links meaning entirely different things in different contexts, like "banken" might mean banks, or couches depending on our query.
 - Possible solutions: We need a way to encode the entire query context! Might actually not be huge problem due to the reachability criterions.

Ideas: Compute similarity scores for traversed web by some metric of similarity of dereferenced documents
similarity score based on string similarity (easy enough, might require some preprocessing of string to remove noise in string)
Start with RL approach that uses contextual bandit (simple linear regression) and learns weights for different structural indicators. 

Exploit hierarchical nature of solid pod: if we go into pod/comments/... we are quite sure of what we'll find. Split pods


Store clusters of documents based on predicate overlap / subject overlap / object overlap. Per cluster store the $k$ most occuring predicates / subjects / objects to know how to cluster new documents.
Score elements of cluster on relevancy based on query and cluster predicates / subject / object overlap.

Idea: Compute characteristic sets for dereferenced data. Cluster these characteristic sets using some clustering mechanism that automatically chooses number of clusters. Track a cache that contains assignments of documents to characteristic set clusters. (Compute characteristic documents) idea is that documents will have similar predicates in them. Like users, posts, ... look at documents with similar file system hierarchy so characterise all documents that have parent .../posts/, achieve ... by adapting the merging from characteristic sets. So if we have characteristic documents pod123/posts/, pod18934/posts/, pod1832/posts with (similar how?) characteristic sets, we merge into .../posts/.
Use characteristic sets for link prioritisation by taking from cache the characteristic set of document, calculating max overlap with query characteristic sets. If not in cache give default priority. 
Characteristic sets can also be used in query planning by checking which characteristic sets we got from cache and their cardinalities

First experiment, look at char set overlap between relevant and irrelevant documents! -->