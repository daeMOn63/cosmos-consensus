
## Creating a release

### Determine version number

Ensure you're on master and up to date with the remote, then git desccribe should returns the latest tag:

```bash
$ git checkout master 
$ git pull origin master
$ git describe
v0.15.1
```

Here our current version is `v0.15.1`, and we want to create the `v0.15.2` release. We'll use this number in the rest of the document.

### Create release

Run `./scripts/fetchai/release.sh` and answer the questions.

This will ask for confirmation to update the `./version/version.go` file, and provide the release version number. Then, it will automatically create and push a new tag to this version, offers the option to view and edit the release notes, and then invoke `goreleaser` which will create the release on Github.

## Next stepsé

To use this new release, both `cosmos-sdk` and `fetchd` need to be updated.

### Update cosmos-sdk

First, update [cosmos-sdk](https://github.com/fetchai/cosmos-sdk) `go.mod` file to reference our new version:

```diff
- replace github.com/tendermint/tendermint => github.com/fetchai/cosmos-consensus v0.15.1
+ replace github.com/tendermint/tendermint => github.com/fetchai/cosmos-consensus v0.15.2
```

then run `go mod tidy` to update the sum file, and publish a new `cosmos-sdk` release following the release guide in this repository.

### Update fetchd

Once `cosmos-sdk` release is published, [fetchd](https://github.com/fetchai/fetchd) `go.mod` file can be updated to change both `cosmos-sdk` and `cosmos-consensus` versions:

```diff
- replace github.com/cosmos/cosmos-sdk => github.com/fetchai/cosmos-sdk v0.15.0
+ replace github.com/cosmos/cosmos-sdk => github.com/fetchai/cosmos-sdk v0.15.1

- replace github.com/tendermint/tendermint => github.com/fetchai/cosmos-consensus v0.15.1
+ replace github.com/tendermint/tendermint => github.com/fetchai/cosmos-consensus v0.15.2
```

then run `go mod tidy` to update the sum file.
