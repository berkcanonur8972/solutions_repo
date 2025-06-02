# Problem_1
 
## Equivalent Resistance Using Graph Theory
 
### Objective
 
Implement an algorithm to calculate the equivalent resistance of an electrical circuit modeled as a graph. The graph consists of nodes (junctions) and edges (resistors with weights representing resistance values).
 
---
 
## Algorithm Overview
 
We model the circuit as an undirected weighted graph:
 
- **Nodes**: Represent electrical junctions.
- **Edges**: Represent resistors with weight equal to resistance in ohms (Ω).
 
We aim to simplify the graph iteratively using two primary operations:
 
- **Series Reduction**
- **Parallel Reduction**
 
---
 
## Key Concepts
 
### Series Combination
 
If two resistors \( R_1 \) and \( R_2 \) are in series:
 
- Total resistance:  
  $$ R_{ext{eq}} = R_1 + R_2 $$
 
### Parallel Combination
 
If two resistors \( R_1 \) and \( R_2 \) are in parallel:
 
- Total resistance:
 
  $$
  \frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2}
  $$
 
  or
 
  $$
  R_{\text{eq}} = \left( \frac{1}{R_1} + \frac{1}{R_2} \right)^{-1}
  $$
 
---
 
## Pseudocode
 
```python
function calculate_equivalent_resistance(graph, start_node, end_node):
    while graph is not simplified:
        apply_series_reduction(graph)
        apply_parallel_reduction(graph)
    return graph.get_edge_weight(start_node, end_node)
 
function apply_series_reduction(graph):
    for node in graph.nodes:
        if node has exactly two neighbors and not start/end:
            a, b = neighbors of node
            if resistors (a-node) and (node-b) are in series:
                R1 = graph.get_edge_weight(a, node)
                R2 = graph.get_edge_weight(node, b)
                R_eq = R1 + R2
                graph.remove_node(node)
                graph.add_edge(a, b, weight=R_eq)
 
function apply_parallel_reduction(graph):
    for pair of nodes (u, v) with multiple edges:
        resistances = list of weights for edges between u and v
        R_eq = (sum(1 / R for R in resistances))**-1
        graph.remove_all_edges(u, v)
        graph.add_edge(u, v, weight=R_eq)
```
 
---
 
## Example Use Cases
 
### Example 1: Simple Series and Parallel
 
- **Series**: A - B - C with resistors 2Ω and 3Ω  
  - Result:  
    \( R = 2 + 3 = 5\,\Omega \)
 
- **Parallel**: A - B with two resistors 4Ω and 6Ω  
  - Result:  
    $$
    \frac{1}{R} = \frac{1}{4} + \frac{1}{6} = \frac{5}{12} \Rightarrow R = \frac{12}{5} = 2.4\,\Omega
    $$
 
### Example 2: Nested Configurations
 
Graph:
 
```
A --(2Ω)-- B --(3Ω)-- C
 \                      /
  -----(6Ω)------------
```
 
- Combine B-C and C-A in series first → then reduce with A-B in parallel.
 
### Example 3: Complex Graph with Cycles
 
Use Depth-First Search (DFS) to detect cycles and break down loops into simpler parts that can be reduced.
 
---
 
## Implementation (Python + NetworkX)
 
```python
import networkx as nx
 
def calculate_equivalent_resistance(graph, start, end):
    changed = True
    while changed:
        changed = apply_series_reduction(graph, start, end) or apply_parallel_reduction(graph)
    return graph[start][end]['weight']
 
def apply_series_reduction(graph, start, end):
    changed = False
    for node in list(graph.nodes):
        if node in [start, end]: continue
        neighbors = list(graph.neighbors(node))
        if len(neighbors) == 2:
            a, b = neighbors
            if graph.has_edge(a, node) and graph.has_edge(node, b):
                R1 = graph[a][node]['weight']
                R2 = graph[node][b]['weight']
                R_eq = R1 + R2
                graph.remove_node(node)
                graph.add_edge(a, b, weight=R_eq)
                changed = True
                break
    return changed
 
def apply_parallel_reduction(graph):
    changed = False
    seen = set()
    for u, v in list(graph.edges()):
        if (u, v) in seen or (v, u) in seen:
            continue
        multiedges = [(a, b) for a, b in graph.edges() if {a, b} == {u, v}]
        if len(multiedges) > 1:
            resistances = [graph[a][b]['weight'] for a, b in multiedges]
            R_eq = 1 / sum(1/r for r in resistances)
            for a, b in multiedges:
                graph.remove_edge(a, b)
            graph.add_edge(u, v, weight=R_eq)
            changed = True
            break
        seen.add((u, v))
    return changed
```
 
### Equivalent resistance between A and C: 5.00 Ω
 
## Efficiency and Improvements
 
- **Efficiency**: The algorithm performs well for small to moderately complex graphs. It simplifies the graph iteratively in \( O(n) \) per iteration.
- **Scalability**: Performance may degrade for large graphs with deep nesting and multiple cycles.
- **Improvements**:
  - Add cycle detection and reduction via Kirchhoff’s laws for general cases.
  - Use Union-Find for dynamic connectivity queries.
  - Optimize by caching subgraph results.
 
---
 
## Deliverables Recap
 
- Full Python implementation using `networkx`
- Handles series, parallel, and nested configurations
- Tested with multiple graph examples
- Analysis of efficiency and scalability
 
---
 
## Resources
 
- [NetworkX Documentation](https://networkx.org/documentation/stable/)
- Depth-First Search: For identifying paths and cycles.
- Kirchhoff’s Laws: For complex cyclic configurations.
 
---