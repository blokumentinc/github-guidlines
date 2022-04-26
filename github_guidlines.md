# Branching

## Quick Legend

<table>
  <thead>
    <tr>
      <th>Instance</th>
      <th>Branch</th>
      <th>Description, Instructions, Notes</th>
    </tr>
  </thead>
  <tbody>
     <tr>
      <td>Master</td>
      <td>master</td>
      <td>Accepts merges from Features/Issues and Hotfixes</td>
    </tr>
    <tbody>
     <tr>
      <td>master/client</td>
      <td>master</td>
      <td>Accepts merges from Master,Features/Issues </td>
    </tr>
    <tr>
      <td>Testing</td>
      <td>stable</td>
      <td>Accepts merges from Develop,Features/Issues and Hotfixes</td>
    </tr>
    <tr>
      <td>Develop</td>
      <td>master</td>
      <td>Accepts merges from Features/Issues and Hotfixes</td>
    </tr>
    <tr>
      <td>Features/Issues</td>
      <td>feat/*</td>
      <td>Always branch off HEAD of Develop</td>
    </tr>
    <tr>
      <td>Hotfix</td>
      <td>hotfix/*</td>
      <td>Always branch off Develop</td>
    </tr>
  </tbody>
</table>

## Main Branches

The main repository will always hold two evergreen branches:

* `master`
* `testing`
* `develop`
* `master/client`

The main branch should be considered `origin/develop` and will be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release. As a developer, you will be branching and merging from `master`.

Consider `origin/master` to always represent the latest code deployed to production. During day to day development, the `stable` branch will not be interacted with.

When the source code in the `develop` branch is stable and has been deployed, all of the changes will be merged into `master` and tagged with a release number.

`testing` branch will be used to get changes `develope`,`hotfix/*` ,`feat/*` and get deployed onto QA environment.

`master/client` is used to provide latest code for specific client. It will take a code from `master` in canse client want latest code and for specific feature it will take from `feat/*` and `hostfix/*`.


## Supporting Branches

Supporting branches are used to aid parallel development between team members, ease tracking of features, and to assist in quickly fixing live production problems. Unlike the main branches, these branches always have a limited life time, since they will be removed eventually.

* For all supprting branch names we must use camel casing.
The different types of branches we may use are:

* Feature branches
* Bug branches
* Hotfix branches

Each of these branches have a specific purpose and are bound to strict rules as to which branches may be their originating branch and which branches must be their merge targets. Each branch and its usage is explained below.

### Feature Branches

Feature branches are used when developing a new feature or enhancement which has the potential of a development lifespan longer than a single deployment. When starting development, the deployment in which this feature will be released may not be known. No matter when the feature branch will be finished, it will always be merged back into the develop branch.

During the lifespan of the feature development, the lead should watch the `develop` branch (network tool or branch tool in GitHub) to see if there have been commits since the feature was branched. Any and all changes to `develop` should be merged into the feature before merging back to `develop`; this can be done at various times during the project or at the end, but time to handle merge conflicts should be accounted for.


* Must branch from: `develop`
* Must merge back into: `develop`
* Branch naming convention: `feature-Name`

#### Working with a feature branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A feature branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b feature-name develop                 // creates a local branch for the new feature
$ git push origin feature-name                        // makes the new feature remotely available
```

Periodically, changes made to `develop` (if any) should be merged back into your feature branch.

```
$ git merge develop                                  // merges changes from develop into feature branch
```

When development on the feature is complete, the lead (or engineer in charge) should merge changes into `develop` and then make sure the remote branch is deleted.

```
$ git checkout develop                               // change to the develop branch  
$ git merge --no-ff feature-id                      // makes sure to create a commit object during merge
$ git push origin develop                            // push merge changes
$ git push origin :feature-id                       // deletes the remote branch
```

### Bug Branches

Bug branches differ from feature branches only semantically. Bug branches will be created when there is a bug on the live site that should be fixed and merged into the next deployment. For that reason, a bug branch typically will not last longer than one deployment cycle. Additionally, bug branches are used to explicitly track the difference between bug development and feature development. No matter when the bug branch will be finished, it will always be merged back into `develop`.

Although likelihood will be less, during the lifespan of the bug development, the lead should watch the `develop` branch (network tool or branch tool in GitHub) to see if there have been commits since the bug was branched. Any and all changes to `develop` should be merged into the bug before merging back to `develop`; this can be done at various times during the project or at the end, but time to handle merge conflicts should be accounted for.

* Must branch from: `develop`
* Must merge back into: `develop`
* Branch naming convention: `bug-name`

#### Working with a bug branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A bug branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b bug-id develop                     // creates a local branch for the new bug
$ git push origin bug-id                            // makes the new bug remotely available
```

Periodically, changes made to `develop` (if any) should be merged back into your bug branch.

```
$ git merge develop                                  // merges changes from develop into bug branch
```

When development on the bug is complete, [the Lead] should merge changes into `develop` and then make sure the remote branch is deleted.

```
$ git checkout develop                               // change to the develop branch  
$ git merge --no-ff bug-name                          // makes sure to create a commit object during merge
$ git push origin develop                            // push merge changes
$ git push origin :bug-name                           // deletes the remote branch
```

### Hotfix Branches

A hotfix branch comes from the need to act immediately upon an undesired state of a live production version. Additionally, because of the urgency, a hotfix is not required to be be pushed during a scheduled deployment. Due to these requirements, a hotfix branch is always branched from a tagged `master` branch. This is done for two reasons:

* Development on the `develop` branch can continue while the hotfix is being addressed.
* A tagged `master` branch still represents what is in production. At the point in time where a hotfix is needed, there could have been multiple commits to `develop` which would then no longer represent production.

* Must branch from: tagged `master`
* Must merge back into: `develop` and `master`
* Branch naming convention: `hotfix-<>`

#### Working with a hotfix branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A hotfix branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b hotfix-bug-name master                  // creates a local branch for the new hotfix
$ git push origin hotfix-bug-name                         // makes the new hotfix remotely available
```

When development on the hotfix is complete, [the Lead] should merge changes into `master` and then update the tag.

```
$ git checkout master                               // change to the master branch
$ git merge --no-ff hotfix-bug-name                       // forces creation of commit object during merge
$ git tag -a <tag>                                  // tags the fix
$ git push origin master --tags                     // push tag changes
```

Merge changes into `develop` so not to lose the hotfix and then delete the remote hotfix branch.

```
$ git checkout develop                               // change to the develop branch
$ git merge --no-ff hotfix-bug-name                       // forces creation of commit object during merge
$ git push origin develop                            // push merge changes
$ git push origin :hotfix-bug-name                        // deletes the remote branch
```