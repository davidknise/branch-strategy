# branch-strategy

A git branching strategy for developing and deploying new features.

## Parallel Development

The goal is that each feature is independently developed and once complete, awaits to be included in a release. This prevents the "develop" or "golden" model, where all developers work on a single branch until there is a "golden build", where everything works. If one feature is broken or delayed, it delays the release of all other features. By independently developing each feature, we can work on multiple features simultaneously and deploy them when they are ready.

## Branch Hierarchy

A repo adhering to this branch strategy will contain the following types of branches:

* **main** - What's currently deployed in production.
  * **release/\<vNext\|hotfix\>** - The "release branch", an intermediate branch to merge multiple features to for a release.
  * **\<work item type\>/\<work item #\>/\<description\>** - The "feature branch", a branch for a single, releasable unit of work.
    * **dev/\<username\>/\<work item #\>/\<description\>** - The "dev branch", which a developer will commit code to.

## Branch Cycle

`main` => `feature` => `dev` => `feature` => `release/vNext` => `main`

1. A `feature` branch should be created off of `main` for a unit of work that is releasable, attached to a work item.
   * A feature branch should be of the format: `<work item type>/<work item #>/<description>`
   * Use the type of a work item followed by the work item number in the branch.
      * This provides additional information about what the work is.
      * By referencing the work item number, it also prevents branch collisions.
      * Examples:
         * issue/1234/my-fix
         * bug/12345/fix-everything
         * task/56789/do-something
         * userstory/8675309/change-the-world
1. One or more "dev" branches should be created off of the feature branch to commit code.
   * A dev branch be of the format: `dev/<username>/<work item #>/<description>`
      * By prefixing your dev branch with the work item number, it's easy to associate what dev branches are associated with what feature branches, as well as other dev branches.
      * If a feature branch is expected to have one branch, use the same description.
   * Multiple `dev` => `feature` branches and pull requests can be created to complete the work.
      * Sending smaller subsets of work is easier to review.
1. Once a `feature` branch is complete, meaning all `dev` branches have been merged, a pull request from the `feature` to `release/vNext` should be created.
1. When there are one or more features ready to release:
   * A release branch should be forked off of main like `release/vNext`
   * Feature branches to pull into the release should pull in changes from the release branch and resolve conflicts
      * Unreleased feature branches should pull from main and resolve conflicts with every release to prevent the branches from getting too far out of sync
   * Each feature branch to release should be pull requested one by one into the release branch
   * Each additional feature branch should re-pull in changes to the release branch and resolve conflicts

## Tenets

1. Every commit in main should be released
1. Every commit in main should be the result of a merge from a release branch
1. Every commit in main should be tagged with the number of the correlating release
   * Format: `v1.2.3`
1. Delete all branches after they are merged.
   * Not deleting branches leads to repo bloat.
   * Keep the repo clean by only saving references to branches not already in main and are still being worked on.

## Disclaimer

This document does not claim any originality.