# Versioning 

This document defines the way in which we work with branches and software versions.

Software versions are defined by branch names of tags following `vX.Y.Z` syntax.
* `X` - a number defining major, tested and stable releases
* `Y` - a number defining tested and table iterations
* `Z` - a number defining tested and table improvement proposals (KIP's, CIP's, etc.)

While `Z` is stakeable, the `Y` and `X` are not, this means that `Z` signifies a count of improvement proposals (features) within iteration `Y`.
For example if `KIP_1`, `KIP_2`, `KIP_3` were implemented then `Z` should equal `3` for a branch name containing all those improvement proposals. 
Every time new `X` or `Y` is released the `Z` should reset to `1`.

Every time a new non-stable release is being worked on, a feature branch following `kip_N_N_N_...` or `cip_N_N_N_...` syntax should be created (kip/cip depending on the improvement proposal type)

For example if developer works on the `KIP_7` and `KIP_32` on the same branch, the feature branch should be named `kip_7_32` where `N` represents a proposal number. Once the feature branch is tested a developer can create a new branch off his feature branch called `v.X.Y.5` for example where `X` and `Y` are numbers depending on the iteration, and `5` implies that this branch contains `5` features including `KIP_7` and `KIP_32`.


## Example Workflow

```
master
  |
  |-->kip_1                      * development of KIP_1 starts
  |    |                         
  |--- | ->kip_2                 * development of KIP_2 starts
  |    |    |                    
  |    \--- | -> v0.0.1          * development of KIP_1 complete
  |         |       |            
  |         |<------/            * KIP_2 must merge latest v0.0.1
  |         |                    * conflicts resolving & testing
  |         \--> v0.0.2          * development of KIP_2 complete
  |                 |            * debugging & testing
  |                 \-> v0.1.0   * iteration 1 is complete
  |                       |      * cleanup, refactoring & testing
  |<--------------------v1.0.0   * release and merge to master
```




