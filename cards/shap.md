Starter ideas for contributing to `shap`:

- move all 16 `numba` njit-AOT compiled functions to c++ via `nanobind`,
additionally move the Cython function to `nanobind`. We accept one PR per
function. Functions need to be well tested (only via the Python
interface), to prevent bugs, especially memory related ones.

- increase the test coverage of our code, the aim is at least 80%
coverage per-file you touch, on PR per file

- update notebooks, see this issue:
https://github.com/shap/shap/issues/3036. One PR per file.

- `shap` currently supports lots of small tree libraries explicitly. In
future releases we want to have this done implicitly, for more
information pls read: github.com/shap/shap/issues/4212. Therefore the
initial plan is to prototype a `TreeEnsemble.from_trees` classmethod, that
generates the object necessary for `shap` to explain the model. Once this
works, we want to have an exhaustive test suit for all currently
supported models with this new behaviour. We'll target scikit-learns
tree structure, see here:
https://scikit-learn.org/stable/auto_examples/tree/plot_unveil_tree_structure.html.
Finally adding `DeprecationWarnings` to a list of models (to be defined)
will round up the feature.

- restructure plotting behaviour, see
https://github.com/shap/shap/issues/3411 for a detailed breakdown. First
actions include getting familiar with the plotting interface, getting to
know the code and then tackling the open issues one by one.

Further ideas, for discussion:

* implement more explainers
* bringing the API closer to `scikit-learn`
