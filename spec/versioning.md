# Versioning 

This document defines the way in which we work with branches and software versions.

Software versions are defined by branch names of tags following `vX.Y.Z` syntax.
* `X` - a number defining major, tested and stable releases
* `Y` - a number defining tested and stable iterations
* `Z` - a number defining tested and stable features - improvement proposals (KIP's, CIP's, etc.)

While `Z` is stakeable, the `Y` and `X` are not, this means that `Z` signifies a count of improvement proposals (features) within iteration `Y`.
For example if `KIP_1`, `KIP_2`, `KIP_3` were implemented then `Z` should equal `3` for a branch name containing all those improvement proposals. 
Every time new `X` or `Y` is released the `Z` should reset to `1`.

Every time a new non-stable release is being worked on, a feature branch following `kip_N_N_N_...` or `cip_N_N_N_...` syntax should be created (kip/cip depending on the improvement proposal type)

For example if developer works on the `KIP_7` and `KIP_32` on the same branch, the feature branch should be named `kip_7_32` where `N` represents a proposal number. Once the feature branch is tested a developer can create a new branch off his feature branch called `v.X.Y.5` for example where `X` and `Y` are numbers depending on the iteration, and `5` implies that this branch contains `5` features including `KIP_7` and `KIP_32`.

To easily figure out what `Z` you should assign simply check what is the latest `Z` for your iteration and add to it number of features you want to push.


### Example Workflow

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

## Testing 

Before any feature branch `vX.Y.*` becomes merged into milestone branch `vX.*.0` or milestone branch merged into release branch milestone branch `v*.0.0` there might be a requirement for debugging and testing. In such case a branch being debugged by a tester should be cloned into a new branch with a suffix `-debug`, e.g. `v0.0.2-debug` or `v0.1.0-debug`.

Successfully tested debug branch with changes can be merged into its original branch after developer who created the original branch is informed about changes. For that purpose a pull-request can be created.

### Example Workflow

```
v0.0.1
  |
  |--> v0.0.1-debug
  |      |
  |      |
  |<-----/ (PR)
```

## Edge Cases

It might happen that old iterations are being worked on in parallel with new iterations, in such case developers working on individual iterations should be keeping the versioning related to their iterations. E.g. push Iteration-1 related changes to `v0.1.Z` and Iteration-2 related changes to `v0.2.Z`.

Code should always be merged with the latest release which is available in the internal Log Book. Developers should never modify branches of other developers as each improvement proposal is aimed to be kept simple and designed for each individual developer to execute on his own.

## Releases

For the purpose of releasing and synchronizing work following `3` branches are created:

* `latest` - branch to which all latest stable changes should be merged by all developers to early on minimize any possible future conflicts

* `dev` - a pre-release branch where code should be merged before it's ready to be shipped to testers and/or clients

* `master`/`main` - branches with restricted access require PR

### Releases Workflow

`latest` -> `KIP_X` -> `vX.Y.Z` -> `latest` -> `dev` -PR-> `master`/`main` 