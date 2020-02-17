---
title: Req doc - Dropping release and changelog from spec file
tags: Requirement document, Fedora
---

# Requirements

* Opt-in
* Opt-out
* Changelog generated must be editable
* Not all commits must appear in the changelog
* Changelog and release fields must be easily "guessable" before making a build
* Must not break current RPM/spec files
* Must not break local builds
* Changes should be minimal for packagers
* A build should be tracable to a specific commit at least as easily as now


# Approaches

## Changelog


### Get the list of build from the build system

The macro would start by getting the commit hash for each of the previous builds so the
corresponding changelog entries can have the associate <version>-<release>.
  - Note: This only covers the <version>-<release> part of the changelog and need to be combined
    with another idea (cf below)
  - (-) Heavy reliance on the build system


### Changelog from last build

The macro starts by getting the old changelog from the last build and only happens to it
the new changes
  - (-) This is breaking the "must be able to edit the changelog" requirement
  - (-) Heavy reliance on the build system


### Annotated git tags

Use annotated git tags to store the changelog information.
* (-) This is not getting ride of a changelog, it just moves it from the spec file into git
* (-) Editing the changelog would mean allowing to remove the git tags, thus leading to potential
      issue in build reproducibility
* (+) The user can be prompted with a pre-filled changelog they would just need to review/adjust before
      doing the tagging operation itself (but see the point above about editing it later on)
* (?) How is the release field handled? (cf ideas below)


### Inclusive Magic Keywords

The commit message contains a commit message to tell the process to include its message in the 
generate changelog.

The generated changelog can be tweaked using other magic keywords: #ignore <commit hash> or
#replace <commit hash> to either remove or replace a commit message from the generate changelog.

* (-) Fragile, parsing commit messages with regex is not nice
* (-) Requires changes to our packaging guidelines as not all commit message will be associable
      to a release, so some entries will not contain the <version>-<release> in the first line.
* (+) Self-contained to git, ie: does not require any information from the build system
* (?) How to regenerate the changelog for the old releases?


### Exclusive Magic Keywords

The commit message contains a commit message to tell the process to exclude a message in the 
generate changelog.

The generated changelog can be tweaked using other magic keywords: #ignore <commit hash> or
#replace <commit hash> to either remove or replace a commit message from the generate changelog.

* (-) Fragile, parsing commit messages with regex is not nice
* (-) Requires changes to our packaging guidelines as not all commit message will be associable
      to a release, so some entries will not contain the <version>-<release> in the first line.
* (+) Self-contained to git, ie: does not require any information from the build system
* (?) How to regenerate the release for the old entries?


### External changelog

The changelog is stored outside of the spec file, in a different, dedicated, file.
* (+) Conflicts in the spec file would no longer occur in the %changelog section
* (-) Conflicts in PRs or within git would occur in this new file
* (-) This is not getting ride of a changelog, it just moves it to a separate file
* (+) No dependency on the build system


### Exclusive Magic Keyword + external changelog

The current RPM changelog is removed from the current spec file and placed into another
file in dist-git.
The macro gets specified the name of the external changelog.
The macro generates the changelog up until the last commit touching the external changelog file.
A magic keyword can be used to ignore a specific commit (#ignore?).

Editing the changelog ends up doing:
  fedpkg generate-changelog
Take the output and put it in the external changelog, tweak it as desired and commit your changes.
The macro will include this external changelog and ignore the entire history before that
commit.

* (+) The external changelog makes it easy for the macro to know when to stop browsing the history
* (+) Edit is fairly straight forward
* (-) Packagers must remember to include the old entries when they touch the external changelog file
* (?) How to get the release of the old entries?


## Release

- Get the release field using the number of tags since the last version change
  - (-) Breaks if two builds are triggered from the same commit
        - Note: this is status quo compared to today
  - (+) Does not rely on the build system


- Get the release field from the annotation of the git tag
  - (-) Fragile: this means parsing using a regex the git annotation to extract the release
        information
  - (-) Breaks if two builds are triggered from the same commit
        - Note: this is status quo compared to today
  - (+) Does not rely on the build system


- Compute the release field from the number of commits since the last version change
  - (-) Breaks if two builds are triggered from the same commit
        - Note: this is status quo compared to today
  - (+) Does not rely on the build system
  - (+) No changes to the guidelines: All changelog entries are linked to a release
  - (+) Easy to reproduce locally
  - (+) Easy to link a certain build with a specific commit
  - (+) Easy to "guess" the next release value before triggering the build


- Compute the release field from the number of commits since the last version change and number 
  of successful builds for this <version>-<commits_number>
  - Note: the build number could be hidden for the first build of the release and only added if
    a specific commit is built twice
  - (+) Allows trigger two builds from the same commit without any manual change
  - (-) Relies a little on the build system
  - (+) No changes to the guidelines: All changelog entries are linked to a release
  - (+) Easy to reproduce locally
  - (+) Easy to link a certain build with a specific commit
  - (+) Easy to "guess" the next release value before triggering the build

