---
layout: page
title: Theses & Dissertation
---


# A new Nested Graph Model for Data Integration 

> Despite graph data gained increasing interest in several fields, no data model suitable for both querying and integrating differently structured graph and (semi)structured data has been currently conceived. The lack of operators allowing combinations of (multiple) graphs in current graph query languages (graph joins), and on graph data structure allowing neither data integration nor nested multidimensional representations (graph nesting) are a possible motivation. In order to make such data integration possible, this thesis proposes a novel model (General Semistructured data Model) allowing the representation of both graphs and arbitrarily nested contents (e.g., one node can be contained by more than just one parent node), thus allowing the definition of a nested graph model, where both vertices and edges may include (overlapping) graphs. We provide two graph joins algorithms (Graph Conjunctive Equijoin Algorithm and Graph Conjunctive Less-equal Algorithm) and one graph nesting algorithm (Two HOp Separated Patterns). Their evaluation on top of our secondary memory representation showed the inefficiency of existing query languages’ query plan on top of their respective data models (relational, graph and document-oriented). In all three algorithms, the enhancement was possible by using an adjacency list graph representation, thus reducing the cost of joining the vertices with their respective outgoing (or ingoing) edges, and by associating hash values to both vertices and edges. As a secondary outcome of this thesis, a general data integration scenario is provided where both graph data and other semistructured and structured data could be represented and integrated into the General Semistructured data Model. A new query language outlines the feasibility of this approach (General Semistructured Query Language) over the former data model, also allowing to express both graph joins and graph nestings. This language is also capable of representing both traversal and data manipulation operators.

## Errata Corrige

[TODO]
