Prompting Strategy — Performance Reflection

The benchmark shows that Few-shot prompting achieved the highest extraction accuracy (2.9/3) and tied with Structured prompting for the highest LLM judge score (3.9/4). However, the improvement over Zero-shot and Structured prompting was marginal.

Structured prompting provided the best overall engineering trade-off, achieving 2.8/3 accuracy, 3.9/4 judge score, 100% parse success, and the lowest p50 latency (2.21s). Its explicit schema and extraction rules provide strong consistency without the additional prompt overhead of Few-shot or CoT.

Zero-shot performed competitively with 2.8/3 accuracy and 3.8/4 judge score, demonstrating that a simple, well-defined prompt can be effective for straightforward extraction tasks.

CoT-style prompting delivered the weakest result (2.7/3) and did not improve judge performance, while adding latency. This suggests that explicit reasoning guidance provides limited value for this relatively simple extraction task.

Conclusion

Structured prompting is the preferred production candidate, offering the best balance of accuracy, reliability, latency, and maintainability. Few-shot is a strong alternative when maximizing accuracy is the priority. Zero-shot remains an efficient baseline, while CoT is better reserved for more complex extraction scenarios where reasoning adds measurable value.

Key takeaway: Prompt complexity did not directly translate into better performance; clear structure and explicit extraction rules provided the strongest overall trade-off.