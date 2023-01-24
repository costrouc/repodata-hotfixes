# repodata-hotfixes
## Changes to package metadata to fix behavior

When packages are created, authors do their best to specify constraints that make their package work.  Sometimes things change, and their constraints are not accurate for making things work.  This results in broken environments.  People need to be able to patch the package metadata long after the packages are built, so that we can prevent conda from creating broken environments.  This repository holds python scripts that generate JSON files, which are then applied on top of the repodata.json index files that are generated from the original package content.

## Things that may require a metadata hotfix:

* changes to features or track_features (removal, addition, change to different names)
* addition or removal of dependencies
* addition or removal of constraints

## Actions that repodata-hotfixes (main.py, r.py and msys2.py) scripts can do:

### Dependency and Constraint updates
Changing dependencies and constraints is the primary reason hotfixes are applied.  Their
may be reasons why you need to change a longstanding package but rebuilding may not be
feasible or perhaps not worth the time.  By changing dependencies and constraints,
the data used to solve for dependencies can be modified and leave the larger ecosystem
unharmed.

NOTE: Changes are implemented that the entire dependency or constraint list (i.e. If someone
changes one out of the ten dependency for a single package, all ten will still should be in the
"patch-instructions" as patching is an overwriting operation).
### Removal
Adding a package to the removal list...
### Revoked
Adding a package to the revoked list does...

## Utility scripts:
### Seeing current hotfixes with `gen-current-hotfix-report.py`:

It can be quite difficult to grok what the hotfix scripts are doing.  The script, `gen-current-hotfix-report.py`, attempts to make it easier to see what the current state of the applied hotfixes looks like.

The script downloads the current repodata.  It then shows you a diff.  Example usage of this script:

```
python gen-current-hotfix-report.py main --subdir linux-64 osx-64 win-64 osx-arm64 linux-ppc64le linux-aarch64 linux-s390x noarch
```

For repeated runs add `--use-cache` to avoid downloading the repodata files.

### Testing hotfixes with `test-hotfix.py`:

The script, `test-hotfix.py`,  downloads the current repodata and runs your instructions against it.  It then shows you a diff.

This useful for testing out changes before they are committed and deployed.  This will show differences in current state of hotfixes
and the ones you are working on.

Example usage of this script:
```
python test-hotfix.py main --subdir linux-64 osx-64 win-64 osx-arm64 linux-ppc64le linux-aarch64 linux-s390x noarch
```

Use the `--color` or `--show-pkgs` options for different outputs.

For repeated runs add `--use-cache` to avoid downloading the repodata files.
