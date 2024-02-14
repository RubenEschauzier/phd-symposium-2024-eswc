## Abstract
<!-- Context      -->
Decentralized environments, like Solid, utilize personal data vaults that require fine-grained access control.
<span class="comment" data-author="RV">Maybe not necessary even here to mention Solid; it's (just) a tech for doing this. We can keep the problem on the conceptual technical level.</span>
In these environments, the number of data vaults can exceed millions, a scale not supported by federated querying algorithms. 
<span class="comment" data-author="RV">What you write is correct; however, it will only be understood by those who already understand. For externals, it's not obvious why there would be millions of data vaults. See if you can slightly change the narrative to explain that one paradigm is to move away from large-scale big personal data with a vertical orientation (a big system has data about one aspect of all people, e.g., Gmail) to a horizontally-oriented large collection of small data entities (all data about one person in one place).</span>
Instead, Link Traversal-based Querying (LTQP) can operate effectively in these environments. 
<span class="comment" data-author="RV">Massive leap 🙂 Rather: do we want to investigate whether LTQP can make a difference here?</span>
LTQP is an integrated querying approach that enables the query engine to begin with zero knowledge of the data to query and discover data sources on the fly.
The ability to implement access control is inherent to this approach, as inaccessible documents can be ignored. 
Furthermore, the lack of prior knowledge on which data sources to query makes querying large decentralized environments feasible.
<span class="comment" data-author="RV">We might need to zoom out a bit here; the detailed arguments can be understood by those who already understand; instead, try to say at a very high level what the problems are. Also, try to move the problems before the proposed solution. There's way too much in the CONTEXT bit; it should be 1–2 sentences, and we should be ready for the need. Talk about solutions in Task.</span>
<!-- Need         -->
However, the absence of prior information on the queried data makes LTQP optimization challenging, resulting in suboptimal query plans. 
Presently, LTQP is employed for client-side querying, where one engine instance services a single client.
Despite this, current engines do not utilize client-specific engine usage patterns to implement personalized query optimization algorithms.
<!-- Task         -->
This paper will outline the intended research approach to implementing personalized query optimization for LTQP. 
<!-- Object       -->
The goal is to enhance query optimization algorithms by analyzing historical query engine usage without relying on other forms of prior information. 
<!-- Findings     -->
Personalized optimization will be based on existing work in SPARQL optimization literature, adapted to LTQP, and aimed at replicating their success in reducing query execution time.
<!-- Conclusion   -->
<!-- Perspectives -->

<span class="comment" data-author="RV">So after completing reading the abstract: get MUCH more quickly to LTQP and the specific problems you want to solve. Context is just a quick hint of why traversal makes sense. Save my horizontal/vertical etc. comments for the intro as a broader justification of why you're doing it.</span>
