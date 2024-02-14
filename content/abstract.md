## Abstract
<!-- Context      -->
Decentralized environments, like Solid, utilize personal data vaults that expose documents that require fine-grained access control.
In these environments, the number of data vaults can exceed millions, a scale not supported by federated querying algorithms. 
Instead, Link Traversal-based Querying (LTQP) can operate effectively in these environments. 
LTQP is an integrated querying approach that enables the query engine to begin with zero knowledge of the data to query and discover data sources on the fly.
The ability to implement access control is inherent to this approach, as inaccessible documents within data vaults can be ignored.
Additionally, by finding data sources as queries are executed, the engine does not need to know in advance where all the possibly millions of decentralized data vaults are located.
<!-- Need         -->
However, the absence of prior information on the queried data makes LTQP optimization challenging, resulting in suboptimal query plans. 
Presently, LTQP is employed for client-side querying, where one engine instance services a single client.
Despite this, current engines do not utilize client-specific engine usage patterns to implement personalized query optimization algorithms.
<!-- Task         -->
This paper will describe the proposed research approach for implementing personalized query optimization techniques, such as caching or learned query optimizers, for LTQP. 
<!-- Object       -->
The objective is to improve query optimization algorithms through the analysis of historical query engine usage, without depending on additional prior information. 
<!-- Findings     -->
Personalized optimization will be based on existing work in SPARQL optimization literature and fundamental database theory, adapted to LTQP, and aimed at replicating their success in reducing query execution time.
<!-- Conclusion   -->
<!-- Perspectives -->
