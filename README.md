# Release Cycle

This file documents how I manage my own release cycles for packages I maintain. Old repos may be outdated.

To start with, `main` is the _development_ branch. There's no need to overcomplicate things with an additional `dev` branch. In addition, next version is always developed in `main`. i.e `main` always points to the latest development effort. This is also important so that PRs can properly reference and auto close issues. When there's a need to publish a patch for an earlier version, you can always branch off of that version's tag.

Version naming follows [Semantic Versioning](https://semver.org/).

Releasing follows [Keep a Changelog](https://keepachangelog.com) for the CHANGELOG.md file format.

## Setup

Setting up the repo:
- Create DEVELOPMENT.md which describes the development cycle and possibly links to this document.
- Create release:* labels (ref: https://github.com/mrahhal/css-theming/labels)
- Create .github/release.yml (ref: https://github.com/mrahhal/css-theming/blob/main/.github/release.yml)
- Create CHANGELOG.md. Will be filling this out after creating each release so that a copy of the CHANGELOG is kept offline independent of GitHub. (ref: https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/blob/main/CHANGELOG.md)
- Add to GitHub repo secrets: NUGET_ORG_API_KEY

Ref:
- https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

## Cycle

The cycle might contain specific steps depending on the stack (dotnet/Node/etc).

- Update the version (updating the version first allows CI to produce proper prerelease packages if necessary):
  - dotnet: Update the version in version.props
  - Node: `npm version [...] --no-git-tag-version`
- Work: Merge PRs, assigning them one of the release:* labels (so that they're picked up by auto generated release notes)
  - Alternatively, if work isn't done in PRs write changes to CHANGELOG.md manually 
- Run the tests locally and make sure all pass
- When ready to publish the new version, create an annonated tag:
  - dotnet: Run `./tag-version` which gets the version from version.props file
  - Manual: `git tag -m vx.y.z vx.y.z`
- Push with tags: `git push --follow-tags` (if this is a new branch you've created locally: `git push origin HEAD -u --follow-tags`)
- CI should run, if all's well proceed to next step
  - If for some reason CI fails for the tagged event: delete the tag locally and remotely, fix the problem, then tag and push again
- Publish packages / Run deployments
  - Either have an action workflow that auto publishes/deploys on a tag
  - Or publish/deploy manually
- Go to GitHub Releases, create new release, select previously pushed tag, auto generate release notes (or copy from manual CHANGELOG.md), update more if necessary
- Copy release content back to CHANGELOG.md if necessary
- Close the relevant milestone if it exists

## Reference repos

dotnet:
- https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination
- https://github.com/mrahhal/MR.AspNetCore.Pagination

Node:
- https://github.com/mrahhal/css-theming
- https://github.com/mrahhal/mr-gtag (in progress)
- https://github.com/mrahhal/mr-scroll (in progress)
