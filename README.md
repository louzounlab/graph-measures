# Topological Graph Features

Topological feature calculators infrastructure.

### C++ code - Motif calculations using GPU
**NEW (07.05.20): An updated motif calculation algorithm lowers the GPU memory usage.**

For installation and running instructions for the C++ code, refer to the [manual](features_algorithms/accelerated_graph_features/Cache%20Accelerated%20Graph%20Features%20Manual.pdf).
In short, for the GPU accelerated version using conda:
1. Move into the path "features_algorithms/accelerated_graph_features/src". 
2. Create the environment that has the requirements: _"conda env create -f env.yml"_ 
3. Activate the new environment: _"conda activate boost"_. Then, make the GPU files: _"make -f Makefile-gpu"_.
4. Now, the accelerated graph features (including the GPU motifs) can be used.

## Calculating Features

**NEW (22.12.19): _features_for_any_graph.py_ - A file for calculating any requested features on a given graph.**

The calculations can still be done using the method below.

The feature calculators work on a gnx instance.
At first we'll define a graph (networkx Graph) and a logger

```python
import networkx as nx
from loggers import PrintLogger

gnx = nx.DiGraph()  # should be a subclass of Graph
gnx.add_edges_from([(0, 1), (0, 2), (1, 3), (3, 2)])

logger = PrintLogger("MyLogger")
```

On that graph we'll want to calculate the topological features.
We can do that in 2 ways:

* Calculate a specific feature.

```python
import numpy as np
from features_algorithms.vertices.louvain import LouvainCalculator

feature = LouvainCalculator(gnx, logger=logger)
feature.build()

mx = feature.to_matrix(mtype=np.matrix)
```

* Calculate a set of features.

```python
import numpy as np
from features_infra.graph_features import GraphFeatures

from features_algorithms.vertices.louvain import LouvainCalculator
from features_algorithms.vertices.betweenness_centrality import BetweennessCentralityCalculator

features_meta = {
  "louvain": FeatureMeta(LouvainCalculator, {"lov"}),
  "betweenness_centrality": FeatureMeta(BetweennessCentralityCalculator, {"betweenness"}),
}

features = GraphFeatures(gnx, features_meta, logger=logger)
features.build()

mx = features.to_matrix(mtype=np.matrix)
```
