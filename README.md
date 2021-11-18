# Release Cycle

This file documents how I manage my own release cycles for packages I maintain. Old repos may be outdated.

`main` is the _development_ branch. There's no need to overcomplicate things with an additional `dev` branch. Major/Minor releases can branch off of `main`.

## Setup

Releasing tries to follow https://keepachangelog.com for the CHANGELOG file format.

Setting up the repo:
- Create DEVELOPMENT.md that describes the development cycle and possibly links to this document.
- Create release:* labels (ref: https://github.com/mrahhal/css-theming/labels)
- Create .github/release.yml (ref: https://github.com/mrahhal/css-theming/blob/main/.github.release.yml)
- Create CHANGELOG.md. Will be filling this out after creating each release so that a copy of the CHANGELOG is kept offline independent of GitHub. (ref: https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/blob/main/CHANGELOG.md)

Ref:
- https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

## Cycle

The cycle might contain specific steps depending on the stack (.Net/Node/etc).

- Branch if necessary
- Update the version (updating the version first allows CI to produce proper prerelease packages if necessary)
  - .Net: Update the version in version.props
  - Node: `npm version [...] --no-git-tag-version`
- Work and merge PRs, assigning them one of the release:* labels
- When ready, checkout `main` and merge your branch
- Create an annonated tag: `git tag -m vx.y.z vx.y.z`
- Push with tags: `git push --follow-tags`
- CI should run, if all's well proceed to next step
- Publish packages / Run deployments
  - Either have an action workflow that auto publishes/deploys on a tag
  - Or publish/deploy manually
- Go to GitHub Releases, create new release, select tag, auto generate release notes
- Copy release content to CHANGELOG.md

## Reference repos

.Net:
- https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination

Node:
- https://github.com/mrahhal/mr-gtag
- https://github.com/mrahhal/css-theming
- https://github.com/mrahhal/mr-scroll
