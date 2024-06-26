## Evaluation Plan
{:#EvaluationPlan}


The evaluation of this work will be done by implementing prototype algorithms on their own, or in combination with other prototypes.

- The prototypes will be built into a [modular open-source LTQP query engine](cite:cites taelman2018comunica). By implementing our approach as modules, other researchers can easily replicate and extend these approaches.
- The primary evaluation method is the benchmark introduced in [](#UsagePatternMethod). The benchmark will simulate varying intensities of observed query patterns to determine the impact this has on performance. Further evaluation on different benchmarks will be included if valuable and compatible with LTQP.

The evaluation of query optimization performance will follow evaluation approaches of previous work on LTQP.
In practice, the following metrics are often used as gauges of performance:

- **Query execution time**, indicating overall query execution efficiency.
- **First *k* result arrival times**, as LTQP is a streaming querying approach, producing first results quickly improves the client experience.
- [**Diefficiency**](cite:cites acosta2017diefficiency), measures the efficiency of result arrival times during query execution. Engines that quickly produce many results are considered better.
- **Result completeness**, as any caching or document pruning strategy could introduce mistakes, result completeness will be verified and ensured during evaluation.

These metrics will be used to compare the state-of-the-art approaches for non-personalized LTQP optimization to our intended personalized optimization algorithms.

For further analysis of the caching approaches in [](#CachingMethod), cache hit rate and overhead will be investigated.
The preceding metrics will provide a clear picture of the effectiveness of personalized query optimization for LTQP.
