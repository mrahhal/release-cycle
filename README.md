# Release Cycle

This file documents how I manage my own release cycles for packages I maintain. Old repos may be outdated.

To start with, `main` is the _development_ branch. There's no need to overcomplicate things with an additional `dev` branch. In addition, next version is always developed in `main`. i.e `main` always points to the latest development effort. When there's a need to publish a patch for an earlier version, you can always branch off of that version's tag.

Version naming follows [Semantic Versioning](https://semver.org/).

Releasing follows [Keep a Changelog](https://keepachangelog.com) for the CHANGELOG file format.

## Setup

Setting up the repo:
- Create DEVELOPMENT.md which describes the development cycle and possibly links to this document.
- Create release:* labels (ref: https://github.com/mrahhal/css-theming/labels)
- Create .github/release.yml (ref: https://github.com/mrahhal/css-theming/blob/main/.github.release.yml)
- Create CHANGELOG.md. Will be filling this out after creating each release so that a copy of the CHANGELOG is kept offline independent of GitHub. (ref: https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/blob/main/CHANGELOG.md)

Ref:
- https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

## Cycle

The cycle might contain specific steps depending on the stack (.Net/Node/etc).

- Update the version (updating the version first allows CI to produce proper prerelease packages if necessary)
  - .Net: Update the version in version.props
  - Node: `npm version [...] --no-git-tag-version`
- Work: Merge PRs, assigning them one of the release:* labels (so that they're picked up by release auto generation)
- When ready to publish the new version: create an annonated tag: `git tag -m vx.y.z vx.y.z`
- Push with tags: `git push --follow-tags`
- CI should run, if all's well proceed to next step
- Publish packages / Run deployments
  - Either have an action workflow that auto publishes/deploys on a tag
  - Or publish/deploy manually
- Go to GitHub Releases, create new release, select tag, auto generate release notes, update more if necessary
- Copy release content to CHANGELOG.md

## Reference repos

.Net:
- https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination

Node:
- https://github.com/mrahhal/mr-gtag
- https://github.com/mrahhal/css-theming
- https://github.com/mrahhal/mr-scroll
