# `Jacc`: Jaccard similarity heatmaps

This is a small `R` package for comparing two vectors of labels for the same set of data points.

`Jacc` compares two labels of cluster assignment per data point (or a vector of ground-truth labels and a clustering vector) `c1` and `c2`, matching groups in each vector to each other while maximising the value of an evaluation metric `obj`.
The evaluation metric `obj` is either `f1` (default), `precision` or `recall`.

Three approaches are used to solve the cluster-cluster (or label-cluster) matching problem. All of them seek to maximise the total value of `obj`.
Approach *(i)* gives 1-to-1 matches, whereby each group in `c1` is matched to a (different) group in `c2`. (In the special case where the number of groups in `c1` is equal to the number of groups in `c2`, this guarantees no unmatched groups.)
Approach *(ii)* uses a relaxed fixed-`c1` matching, whereby each group in `c1` is matched to the group in `c2` that maximises `obj` value of the match. This can result in 1-to-many matches.
Approach *(iii)* uses a relaxed fixed-`c2` matching, which mirrors approach *(ii)*.

If `c1` is in fact a vector of ground-truth labels (or manual annotation of each data point), there may be de-facto unlabelled data points in the original data.
`Jacc` accepts an optional `unassigned` parameter, which is a vector of the labels given to data points which don't belong to an annotated population.
If specified, the unassigned groups in `c1` are left out of the evaluation: points that are unassigned are ignored in constructing the contingency tables for each match and groups in `c2` may not be matched to these unassigned points.

In addition to evaluation results, a heatmap showing agreement between `c1` and `c2` and agreement between the different matching approaches is produced.

### Sample code

The package includes sample inputs that allow you to easily test the functionality described above.

```
c1 <- readRDS(system.file('extdata', 'c1.RDS', package = 'Jacc'))
c2 <- readRDS(system.file('extdata', 'c2.RDS', package = 'Jacc'))

j <- Jacc::Jacc(c1, c2, obj = 'f1', unassigned = 'unassigned', c1_name = 'Gating', c2_name = 'FlowSOM', verbose = TRUE)
j$Plot

```

### Reference

Implementation of 1-to-1 (bijective) mapping via the Hungarian assignment method using `clue::solve_LSAP` is adopted from Lukas Weber's implementation in a benchmarking study (see `LICENSE`).

This is the paper:

Weber, L. and Robinson, M., 2016. Comparison of clustering methods for high-dimensional single-cell flow and mass cytometry data. Cytometry Part A, 89(12), pp.1084-1096.

### Support

This tool is far from perfect!
If you'd like to report a bug or suggest a new feature, contact me at *david.novak@ugent.be*.