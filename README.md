# Release Cycle

This file documents how I manage my own release cycles for packages I maintain. Old repos may be outdated.

To start with, `main` is the _development_ branch. There's no need to overcomplicate things with an additional `dev` branch. In addition, next version is always developed in `main`. i.e `main` always points to the latest development effort. This is also important so that PRs can properly reference and auto close issues. When there's a need to publish a patch for an earlier version, you can always branch off of that version's tag.

Version naming follows [Semantic Versioning](https://semver.org/).

Releasing follows [Keep a Changelog](https://keepachangelog.com) for the CHANGELOG.md file format.

## Setup

Setting up the repo:

- Use a template if possible: [template-dotnet-package](https://github.com/mrahhal/template-dotnet-package), [template-roslyn-analyzer](https://github.com/mrahhal/template-roslyn-analyzer)
- Create DEVELOPMENT.md which describes the development cycle and possibly links to this document.
- Create CHANGELOG.md. Will be filling this out after PRs/commits. After each release, we can optionally copy the release's changelog content to GitHub releases. (ref: https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/blob/main/CHANGELOG.md)
- Add to GitHub repo secrets the relevant secrets: i.e NUGET_TOKEN, NPM_TOKEN, etc

## Cycle

The cycle might contain specific steps depending on the stack (dotnet/Node/etc).

- Update the version (updating the version first allows CI to produce proper prerelease packages if necessary):
  - dotnet: Update the version in version.props
  - Node: `npm version [...] --no-git-tag-version`
- Work:
  - Merge PRs, making sure they update the changelog where appropriate
  - Alternatively, if work isn't done in PRs write changes to the changelog directly in the commit
- Run the tests locally and make sure all pass
- When ready to publish the new version, polish the changelog (refer to the Changelog section below), then create an annonated tag:
  - dotnet: Run `./scripts tag-version` which gets the version from version.props file
  - Manual: `git tag -m vx.y.z vx.y.z`
- Push with tags: `git push --follow-tags` (if this is a new branch you've created locally: `git push origin HEAD -u --follow-tags`)
- CI should run, if all's well proceed to next step
  - If for any reason CI fails for the tagged event, delete the tag locally and remotely, fix the problem, then tag and push again
- Publish packages / Run deployments
  - Either have an action workflow that auto publishes/deploys on a tag
  - Or publish/deploy manually
- Go to GitHub Releases, create a new release, select previously pushed tag, then copy the version's changelog CHANGELOG.md
- Close the relevant milestone if it exists

## Changelog

The CHANGELOG.md file will be filled as we work towards a release. No change should be merged without it being recorded in this file.

Usually when a release is ready, you'd edit the changelog (strip "Unreleased", add a date, make sure the diff link is updated) and from this commit you tag and publish.

More detailed info about a change should be added in the issue or PR, not in the changelog.

Here's a template:

```md
# Changelog

All notable changes to this project will be documented in this file.

Make sure to always view this file from the main branch to get an up to date changelog.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## Unreleased: 1.1.0

[**Full diff**](https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/compare/v1.0.3...HEAD)

### Added

- Support nested properties when defining a keyset ([#19](https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/pull/23) by [@mrahhal](https://github.com/mrahhal))

## 1.0.3 - 2022-06-16

[**Full diff**](https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/compare/v1.0.2...v1.0.3)

This change introduces support for NRTs, lorem ipsum. You can add optional general text here.

### Improved

- Generate expressions to use sql parameters instead of constants ([#19](https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/issues/19) by [@mrahhal](https://github.com/mrahhal))
```

The list of change types (in this order):

- Fixed
- Improved (improvements to existing functionality, i.e perf/etc)
- Added (new features)
- Changed (breaking changes to existing functionality)
- Deprecated
- Removed
- Other

Ref:

https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination/blob/main/CHANGELOG.md

## Reference repos

dotnet:

- https://github.com/mrahhal/template-dotnet-package 🟠 (needs update to release.yml and GH labels)
- https://github.com/mrahhal/template-roslyn-analyzer 🟠 (needs update to release.yml and GH labels)
- https://github.com/mrahhal/MR.EntityFrameworkCore.KeysetPagination ✅
- https://github.com/mrahhal/MR.AspNetCore.Pagination 🟠 (needs update to release.yml and GH labels)

Node:

- https://github.com/mrahhal/css-theming ✅
- https://github.com/mrahhal/mr-gtag ❌
- https://github.com/mrahhal/mr-scroll ❌
