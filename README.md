In yarn workspaces, if one package depends on another package with bin files (and present as a package in the workspace), the bin file is symlinked to the dependent package so it can be run. This behaviour is broken when PnP is turned on. The bin file is no longer symlinked (which is expected as it should use the PnP resolver to find them), but this doesn't happen.

Example workspaces without pnp:

```sh
# bootstrap
cd ./without-pnp
yarn

cd ./packages/package-a
## bin files from `build-script` is symlinked in so we can use them here
ls node_modules/.bin

yarn run build ## runs fine
```

Example workspaces with pnp:

```sh
# bootstrap
cd ./with-pnp
yarn

cd ./packages/package-a
## bin files are not symlinked as expected, as doing `yarn run [cmd]` should use the pnp resolver
ls node_modules/.bin

yarn run build ## but it doesn't resolve correctly and can't find the bin file
```
