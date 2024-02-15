## Problem Statement and Contributions
{:#ProblemStatementandContributions}

Building upon the existing work in [](#LiteratureReview), this thesis will use personalized query optimization to overcome the performance issues outlined in [](#introduction).
Personalized query optimization adapts the optimization procedure to the client-specific usage patterns of the engine. 
As such, the engine will keep a state that stores optimization-relevant information.
<!-- Personalized query optimization involves caching auxiliary data structures, (intermediate) results, and training a client-specific learned query optimizer. -->
The hypothesizes underlying this work are: 

 - **Hypothesis 1:** 
 
 Query engine users exhibit patterns in the usage of decentralized environments, which are expressed in correlated query sequences. These patterns can be identified from the literature and modeled.
 - **Hypothesis 2:** Personalized query engines can automatically extract and leverage client-specific query patterns to improve query execution times by an order of magnitude compared to non-personalized query engines. 
 
Before any work on personalized query optimization can proceed, the different real-world client usage patterns must be identified.
These patterns can include but are not limited to, application data requirements, query requirements for different applications, and data update frequency.
After the identification of query patterns, they should be translated to an extensive benchmark that can validate the performance of personalized query optimization algorithms in real-world applications.
As such, we define research question I.

- **RQ I:** How do client-specific query patterns manifest in real-world usage scenarios, and how can we accurately capture and represent them within a benchmark?

In this thesis two avenues for personalized will be explored: first is the use of caching auxiliary data structures or (intermediate) results, and second is the use of learned query optimizers.
<!-- Following the definition of a benchmark, a natural candidate for validating the possibility of automatically extracting and using query patterns is caching. -->
In the presence of patterns in query sequences, effective caching can achieve high hit rates and reuse expensive computations. 
Two natural candidates for cached content are intermediate results sets for queries or auxiliary data structures that improve query planning.
However, LTQP uses the [_cMatch_ criterion](cite:cites hartig2012foundations) to extract links to dereference. This means what links are extracted and dereferenced depends on what predicates are used in the query.
As a consequence, queries with overlapping sub-BGPs, but different sets of predicates will dereference different documents during query execution.
Simply reusing the result set for the overlapping sub-BGPs of one query on another can generate wrong results, thus it is important to evaluate the effectiveness of caching strategies under these conditions.
This leads to research questions II & III.

- **RQ II:** Can (intermediate) result caching be effectively utilized during Link Traversal-based Query Processing to enhance query execution performance, when the queried subweb of data changes between queries?
- **RQ III:** How does caching auxiliary data structures during Link Traversal-based Query Processing impact the performance of query execution?

Finally, learned optimizers are a promising candidate for extracting usage patterns from sequences of queries.
Statistical learning methods are built to approximate arbitrary data-generating processes from samples of data.
Formulating the client usage patterns as a data-generating process, and the queries as samples from this process, statistical learning fits our problem perfectly.
However, learning methods can be both data and compute expensive.
As the learned optimizer will be trained in an online scenario, we must design any solutions with data and compute efficiency in mind.
This gives us research question IV:


- **RQ IV:** Does the query performance benefit of training a query optimizer during query execution outweight the model training cost?

