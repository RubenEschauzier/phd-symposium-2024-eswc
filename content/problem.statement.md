## Problem Statement and Contributions
{:#ProblemStatementandContributions}

Building upon the existing work in [](#LiteratureReview), this thesis will use personalized query optimization to overcome the performance issues outlined in [](#introduction).
Personalized query optimization adapts the optimization procedure to the client-specific query usage patterns of the engine. 
As such, the engine will keep a state that stores optimization-relevant information.
<!-- Personalized query optimization involves caching auxiliary data structures, (intermediate) results, and training a client-specific learned query optimizer. -->
The hypothesis underlying this work is: 

 - **Hypothesis 1:** Personalized query engines can improve query execution times by an order of magnitude compared to non-personalized query engines by leveraging client-specific query patterns to improve query optimization. 
 
Before any work on personalized query optimization can proceed, the different real-world client query usage patterns must be identified.
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

- **RQ II:** Can (intermediate) result caching be effectively utilized during Link Traversal-based Query Processing to enhance query execution performance when the queried subweb of data changes between queries?
- **RQ III:** How does caching auxiliary data structures during Link Traversal-based Query Processing impact the performance of query execution?

Our caching approaches will build upon and extend methods introduced in [](#LiteratureReview). The primary challenge to overcome in LTQP is the dynamic nature of the queried data, as it can differ between queries, influencing cache validity.

Finally, learned optimizers are a promising candidate for extracting query usage patterns from sequences of queries.
Statistical learning methods are built to approximate arbitrary data-generating processes from samples of data.
Formulating the client query usage patterns as a data-generating process, and the queries as samples from this process, statistical learning fits our problem perfectly.
However, learning methods can be both data and compute expensive and require the exploration of sub-optimal query plans to learn the entire optimization space.
As the learned optimizer will be trained in an online scenario, we must design any solutions with data and compute efficiency in mind.
This gives us research questions IV & V:

- **RQ IV** Can training a query optimizer lead improved query performance in Link Traversal-based Query Processing? 
- **RQ V:** Does the query performance benefit of training a query optimizer during query execution outweight the model training cost?

To answer these questions, proposed methods will build upon existing literature for both learned cardinality and join plan estimation. From this foundation data and compute efficiency approaches will be included to facilitate the usage of learned estimation for LTQP. 