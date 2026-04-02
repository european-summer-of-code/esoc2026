Also see [`shap` project ideas page](https://github.com/shap/shap/wiki/ESoC-2026-Project-Ideas)

## Warm-up Projects and Application Preconditions
These projects can be used to get to know the SHAP library. If you want to apply for the European Summer of Code 2026 full projects, please make sure to file at least one PR addressing the warm-up projects or any other [good first issue](https://github.com/shap/shap/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22). We expect potential applicants to communicate before tackling issues and filing PRs. On easier issues (see difficulty ratings here) a quick note is enough, but we expect more detailed implementation plan for the harder ones. We also expect disclosure of AI usage, see our [AI Usage Policy](https://github.com/shap/shap/blob/master/CONTRIBUTING.md#ai-usage-policy).

### Increase test coverage of SHAP
SHAP currently sits at around ~65% test coverage which is quite low. There is some dead code in the repository, which should be identified, flagged and excluded from test coverage reports. Furthermore we want to increase the test coverage as far as possible. 
We accept one PR per file. See this issue for a detailed breakdown of files where we need more tests: https://github.com/shap/shap/issues/3690.

**Difficulty Rating**: Easy<br>
**Potential Mentors**: Tobias Pitters

### Restructure Plotting Behaviour
We want to restructure the plotting behaviour in SHAP, see
https://github.com/shap/shap/issues/3411 for a detailed breakdown. First
actions include getting familiar with the plotting interface, getting to
know the code and then tackling the open issues one by one.
This will involve some coordination for merges, since we might need to update multiple 
files at once to keep interfaces consistent.

**Difficulty Rating**: Easy/Medium<br>
**Potential Mentors**: Tobias Pitters

## Full ESoC 2026 Projects
### Move performance critical functions from numba to C++
Currently we have 15 functions that use numba's `njit` functionality to compile those just-in-time.
This works reasonably well but we have had problems with numba recently, namely on our MacOS CI jobs and the delay
in supporting new python versions quickly. Therefore we want to move those functions to C++ via [nanobind](https://nanobind.readthedocs.io/en/latest/) and make sure that we match the speed of numba to get rid of the `numba/llvmlite` dependency for SHAP. We need to test this properly, to make sure no memory errors appear.
Furthermore, we have one `Cython` function, which should be moved to C++ as well.

The goal is to move all of the following functions to C++:
  - [`_compute_grey_code_row_values`](https://github.com/shap/shap/blob/master/shap/explainers/_exact.py#L227)                                                                          
  - [`_compute_grey_code_row_values_st`](https://github.com/shap/shap/blob/master/shap/explainers/_exact.py#L261)                                                                       
  - [`lower_credit`](https://github.com/shap/shap/blob/master/shap/explainers/_partition.py#L768)                                                                                       
  - [`_single_delta_mask`](https://github.com/shap/shap/blob/master/shap/maskers/_tabular.py#L202)                                                                                      
  - [`_delta_masking`](https://github.com/shap/shap/blob/master/shap/maskers/_tabular.py#L214)                                                                                          
  - [`_jit_build_partition_tree`](https://github.com/shap/shap/blob/master/shap/maskers/_image.py#L175)                                                                                 
  - [`_pt_shuffle_rec`](https://github.com/shap/shap/blob/master/shap/utils/_clustering.py#L51)                                                                                         
  - [`delta_minimization_order`](https://github.com/shap/shap/blob/master/shap/utils/_clustering.py#L78)                                                                                
  - [`_reverse_window`](https://github.com/shap/shap/blob/master/shap/utils/_clustering.py#L93)                                                                                         
  - [`_reverse_window_score_gain`](https://github.com/shap/shap/blob/master/shap/utils/_clustering.py#L101)
  - [`_mask_delta_score`](https://github.com/shap/shap/blob/master/shap/utils/_clustering.py#L118)                                                                                      
  - [`_build_fixed_single_output`](https://github.com/shap/shap/blob/master/shap/utils/_masked_model.py#L352)                                                                           
  - [`_build_fixed_multi_output`](https://github.com/shap/shap/blob/master/shap/utils/_masked_model.py#L377)                                                                            
  - [`_init_masks`](https://github.com/shap/shap/blob/master/shap/utils/_masked_model.py#L422)                                                                                          
  - [`_rec_fill_masks`](https://github.com/shap/shap/blob/master/shap/utils/_masked_model.py#L434)                                                                                      
  - [`_exp_val`](https://github.com/shap/shap/blob/master/shap/explainers/_kernel_lib.pyx#L13) (Cython)  

Optionally, we could move the existing code in [`_cext`](https://github.com/shap/shap/tree/master/shap/cext) (C API with `PyObject` overhead) to `nanobind` as well to provide a more modern approach for C++ extensions.

**Expected Time**: 200-250 hours<br>
**Difficulty Rating**: Hard<br>
**Required Skills**: C++, Python<br>
**Potential Mentors**: Tobias Pitters

### Unify SHAP value calculation from Tree-based Models

SHAP currently supports lots of small tree libraries explicitly, see [here](https://github.com/shap/shap/blob/master/shap/explainers/_tree.py#L1027-L1241). We previously had lots of problems with a couple of these libraries, effectively blocking our CI, see [this issue](https://github.com/shap/shap/issues/4212) for further details. There is even interest in adding more models (see [here](https://github.com/shap/shap/issues/4164)).
Currently SHAP creates a [`TreeEnsemble`](https://github.com/shap/shap/blob/master/shap/explainers/_tree.py#L921) from all those different models, which is then used to calculate the SHAP values efficiently. In order to not support more and more libraries, we plan to provide a `TreeEnsemble.from_trees` classmethod, with a well defined and communicated interface such that users or package maintainers are able to provide a way to generate the `TreeEnsemble` object and SHAP values from there.
Therefore the
initial plan is to prototype a `TreeEnsemble.from_trees` classmethod, that
generates the object necessary for SHAP to explain the model. Once this
works, we want to have an exhaustive test suite for all currently
supported models with this new behaviour. We'll target scikit-learn's
tree structure, see here:
https://scikit-learn.org/stable/auto_examples/tree/plot_unveil_tree_structure.html.
Finally adding DeprecationWarnings to a list of models (to be defined)
will round out the feature.

Goals:
 - prototype a `TreeEnsemble.from_trees` classmethod
 - thoroughly test the method against all currently supported tree models
 - build a notebook, showing how to calculate SHAP values from currently supported models using this new functionality
 - deprecate a list of currently supported models (smaller libraries like `gpboost`, `ngboost`, etc.)

**Expected Time**: 200 hours<br>
**Difficulty Rating**: Medium<br>
**Required Skills**: Python, familiarity with tree-based ML models and scikit-learn<br>
**Potential Mentors**: Tobias Pitters
