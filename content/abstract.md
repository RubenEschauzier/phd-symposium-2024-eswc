## Abstract
<!-- Context      -->
Decentralized environments, like Solid, utilize personal data vaults that require fine-grained access control.
In these environments, the number of data vaults can exceed millions, a scale not supported by federated querying algorithms. 
Instead, Link Traversal-based Querying (LTQP) can operate effectively in these environments. 
LTQP is an integrated querying approach that enables the query engine to begin with zero knowledge of the data to query and discover data sources on the fly.
The ability to implement access control is inherent to this approach, as inaccessible documents can be ignored.<span class="comment" data-author="RT">Up until now, you hadn't mentioned yet that these pods expose documents.</span> 
<span class="comment" data-author="RT">Not sure I understand the next sentence. Can you rephrase? Isn't it the discovery aspect of LTQP that makes this querying feasible?</span>
Furthermore, the lack of prior knowledge on which data sources to query makes querying large decentralized environments feasible.
<!-- Need         -->
However, the absence of prior information on the queried data makes LTQP optimization challenging, resulting in suboptimal query plans. 
Presently, LTQP is employed for client-side querying, where one engine instance services a single client.
Despite this, current engines do not utilize client-specific engine usage patterns to implement personalized query optimization algorithms.
<!-- Task         -->
This paper will outline the intended research approach to implementing personalized query optimization for LTQP. <span class="comment" data-author="RT">Maybe you can briefly hint towards a possible optimization? "such as..."</span>
<!-- Object       -->
The goal is to enhance query optimization algorithms by analyzing historical query engine usage without relying on other forms of prior information. 
<!-- Findings     -->
Personalized optimization will be based on existing work in SPARQL optimization literature <span class="comment" data-author="RT">Perhaps also more fundamental db theory?</span>, adapted to LTQP, and aimed at replicating their success in reducing query execution time.
<!-- Conclusion   -->
<!-- Perspectives -->
