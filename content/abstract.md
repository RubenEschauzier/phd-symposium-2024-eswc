## Abstract
<!-- Context      -->
<!-- Decentralized environments, like Solid, utilize personal data vaults that expose documents that require fine-grained access control.
In these environments, the number of data vaults can exceed millions, a scale not supported by federated querying algorithms. 
Instead, Link Traversal-based Querying (LTQP) can operate effectively in these environments. 
LTQP is an integrated querying approach that enables the query engine to begin with zero knowledge of the data to query and discover data sources on the fly.
The ability to implement access control is inherent to this approach, as inaccessible documents within data vaults can be ignored.
Additionally, by finding data sources as queries are executed, the engine does not need to know in advance where all the possibly millions of decentralized data vaults are located. -->
<!-- Need         -->
<!-- However, the absence of prior information on the queried data makes LTQP optimization challenging, resulting in suboptimal query plans. 
Presently, LTQP is employed for client-side querying, where one engine instance services a single client.
Despite this, current engines do not utilize client-specific engine usage patterns to implement personalized query optimization algorithms. -->
<!-- Task         -->
<!-- This paper will describe the proposed research approach for implementing personalized query optimization techniques, such as caching or learned query optimizers, for LTQP.  -->
<!-- Object       -->
<!-- The objective is to improve query optimization algorithms through the analysis of historical query engine usage, instead of depending on additional prior information.  -->
<!-- Findings     -->
<!-- Personalized optimization will be based on existing work in SPARQL optimization literature and fundamental database theory, adapted to LTQP, and aimed at repeating their success in reducing query execution time. -->
<!-- Conclusion   -->
<!-- Perspectives -->

<!-- Context      -->
Current querying approaches lack the capability to accommodate the scale of decentralization envisioned for the presently centralized web. 
This decentralization would create numerous small data sources instead of a concentration of a few large ones.
Link Traversal-based Query Processing (LTQP) is a promising candidate for querying this highly decentralized environments.
LTQP is an integrated querying approach that enables the query engine to execute queries with zero knowledge of the queried data and discover data sources on the fly.
<!-- Need         -->
However, as the engine does not know in advance what data will be queried, creating an optimized query plan before executing the query is challenging.
Presently, LTQP is employed for client-side querying, where one engine instance services a single client.
Despite this, current engines do not utilize client-specific engine usage patterns to implement personalized query optimization algorithms.
<!-- Task         -->
This paper will describe the proposed research approach for implementing personalized query optimization techniques, such as caching or learned query optimizers, for LTQP. 
<!-- Object       -->
The objective is to improve query optimization algorithms through the analysis of historical query engine usage, instead of depending on additional prior information. 
<!-- Findings     -->
Personalized optimization will be based on existing work in SPARQL optimization literature and fundamental database theory, adapted to LTQP, and aimed at repeating their success in reducing query execution time.
<!-- Conclusion   -->
As a result, query engines will gain the capability to query large decentralized environments, thereby enabling applications to function within this emerging decentralized web landscape.
<!-- Perspectives -->

<!-- 


Current querying approaches do not support the scale of decentralization of the currently centralized web.
The resulting decentralized environment would spread personal, possibly access-controlled data, over many smaller sources.
Link Traversal-based Query Processing (LTQP) is an approach that could deal with this level of decentralization of access-controlled data.
Current
More focus on personal
Current federation algorithms do not support the scale of decentralization of centrlaized web now. 
Personal data is spread over many smaller sources.
In this environment, LTQP becomes interesting as data is spread and personal + other reasons so LTQP again interesting
LTQP still slow at this scale
As we're client-side we do optimization to make more scalable.
When moving to decentrilize current centralized environments we need query approach that do many sourcre queries with access control
This scale not supported by current query algorithms (why?)
We propose to investigate use of LTQP as it solves these issues .... in this way
(use documents consistently)
Docuemnts may be access controlled, which can be taken into account during query processing.
Challenge in this space is optimizing query plans without any prior information.  -->