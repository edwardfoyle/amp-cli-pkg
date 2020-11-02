# amp-cli-pkg

Contains scripts for packaging the Amplify CLI as a native binary

## Packaging instructions
1. If packaging the latest version of the CLI on NPM, skip to step 6
2. Start verdaccio locally using https://github.com/aws-amplify/amplify-cli/blob/68315350e52ff9db4c9bffeb6254b70894420333/.circleci/config.base.yml#L96-L99
3. In your local Amplify CLI repo, replace [this line](https://github.com/aws-amplify/amplify-cli/blob/master/package.json#L31) with `"publish-to-verdaccio": "lerna publish --yes --no-git-tag-version --no-commit-hooks --no-push --exact --dist-tag=latest --conventional-commits",`
4. Make sure you have built the changes to the Amplify CLI that you want to include (using `yarn dev-build`)
5. Run `yarn publish-to-verdaccio`
  a. This will update package.json and CHANGELOG.md files in the repo. This can be reverted with `git checkout -- packages/*/package.json packages/*/CHANGELOG.md`
6. Run `yarn && yarn cli-all` in this repository
7. The binaries will be compiles to the `out` directory

To build a new version, you will need to delete the Verdaccio cache in order to publish a new version to Verdaccio.
To ensure no cross-contamination from previous versions, it is best to run `yarn pkg-clean` before running `yarn cli-all` again
To package a different tag / version of the CLI, update the CLI dependency in pkg/package.json

## Uploading instructions
1. Go to https://github.com/aws-amplify/amplify-cli/releases/new to create a new release on GitHub
2. Attach the 3 binaries in the `out` directory to the release. DO NOT RENAME THEM!
3. Tag the release as `v<version>-alpha.0` where `<version>` is the semver string returned by running `version` with the packaged CLI being uploaded
  a. If there is already a published version, increment the 0 at the end of the release tag
4. For release title enter `v4.33.0-alpha.0 (NOT FOR PRODUCTION)`
5. For the description enter:
```
NOT FOR PRODUCTION

This is an initial release of the CLI packaged as a native binary. It is not intended for production at this point.
```
6. Do NOT check the "this is a pre-release" checkbox
7. Once the files are done uploading (it can take a while), click "Update release"