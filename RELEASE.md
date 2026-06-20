# How to Release kuadrant-backstage-plugin

To release a version `vX.Y.Z` of the `kuadrant-backstage-plugin` on GitHub and npm, follow these steps:

## Minor Release

A minor release (e.g., `0.2.0`) is made from a new release branch created from `main`.

### 1. Create the Release Branch

```shell
git checkout main
git pull upstream main
git checkout -b release-X.Y
git push upstream release-X.Y
```

### 2. Prepare the Release Commit

- Remove the `-dev` suffix from the `version` field in both plugin `package.json` files:
  - `plugins/kuadrant/package.json`
  - `plugins/kuadrant-backend/package.json`
- Open a pull request targeting the `release-X.Y` branch.

### 3. Tag the Release

Once the release PR has been merged, create and push the tag:

```shell
git checkout release-X.Y
git pull upstream release-X.Y
git tag -a vX.Y.Z -m "vX.Y.Z"
git push upstream vX.Y.Z
```

### 4. Create the Release on GitHub

- [Create a new GitHub release](https://github.com/Kuadrant/kuadrant-backstage-plugin/releases) from the tag you just pushed.
- Use auto-generated release notes.

### 5. Publish to npm (Automatic)

When the GitHub Release is published, the [Publish workflow](https://github.com/Kuadrant/kuadrant-backstage-plugin/actions/workflows/publish.yml) is triggered automatically. It publishes three npm packages with the `latest` dist-tag:

- [`@kuadrant/kuadrant-backstage-plugin-frontend`](https://www.npmjs.com/package/@kuadrant/kuadrant-backstage-plugin-frontend)
- [`@kuadrant/kuadrant-backstage-plugin-backend`](https://www.npmjs.com/package/@kuadrant/kuadrant-backstage-plugin-backend)
- [`@kuadrant/kuadrant-backstage-plugin-backend-dynamic`](https://www.npmjs.com/package/@kuadrant/kuadrant-backstage-plugin-backend-dynamic)

### 6. Bump to the Next Development Version

- Update the `version` field in both plugin `package.json` files to the next version with a `-dev` suffix (e.g., `0.3.0-dev`).
- Create a PR to merge these changes into `main`.

## Patch Release

A patch release (e.g., `0.1.1`) is made from an existing `release-X.Y` branch. Ensure the release branch already contains the patches backported from `main` before starting.

### 1. Prepare the Release Commit

- Bump the `version` field in both plugin `package.json` files to the patch version (e.g., `0.1.0` → `0.1.1`):
  - `plugins/kuadrant/package.json`
  - `plugins/kuadrant-backend/package.json`
- Open a pull request targeting the `release-X.Y` branch.

### 2. Tag, Release, and Publish

Once the release PR has been merged, follow [steps 3–5 from Minor Release](#3-tag-the-release) to tag, create the GitHub release, and publish to npm.

## Publishing a Dev Build

You can trigger the [Publish workflow](https://github.com/Kuadrant/kuadrant-backstage-plugin/actions/workflows/publish.yml) manually via `workflow_dispatch`. In this case:
- The version is derived from the current `package.json` version plus the short Git SHA (e.g., `0.0.2-dev-abc1234`).
- The package is published with the `dev` dist-tag.
