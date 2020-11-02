# amp-cli-pkg

Contains scripts for packaging the Amplify CLI as a native binary

## Instructions
1. If packaging the latest version of the CLI on NPM, skip to step 5
2. Start verdaccio locally using https://github.com/aws-amplify/amplify-cli/blob/68315350e52ff9db4c9bffeb6254b70894420333/.circleci/config.base.yml#L96-L99
3. In your local Amplify CLI repo, replace [this line](https://github.com/aws-amplify/amplify-cli/blob/master/package.json#L31) with `"publish-to-verdaccio": "lerna publish --yes --no-git-tag-version --no-commit-hooks --no-push --exact --dist-tag=latest --conventional-commits",`
4. Run `yarn publish-to-verdaccio`
5. Run `yarn && yarn cli-all` in this repository
6. The binaries will be compiles to the `out` directory

To build a new version, you will need to delete the Verdaccio cache in order to publish a new version to Verdaccio.
To ensure no cross-contamination from previous versions, it is best to run `yarn pkg-clean` before running `yarn cli-all` again
To package a different tag / version of the CLI, update the CLI dependency in pkg/package.json