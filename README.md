# adorsys-containers/config

configuration meta repository do synchronize various files across
multiple repositories inside the adorsys-containers organisation.

To archive this goal, we are using [voxpupuli/modulesync](https://github.com/voxpupuli/modulesync).

## How to use it

```bash
git clone https://github.com/adorsys-containers/config.git
cd modulesync_config
git checkout $(git tag --list | sort -V | tail -n1) # checkout latest tag
bundle install
bundle exec msync help update
```

## Examples

### module sync one specific module

```bash
bundle exec msync update -f {module_name} --message "modulesync $(git describe)"
```

### module sync one module and review changes before submitting changes

```bash
bundle exec msync update -f {module_name} --noop
cd modules/{module_name}
# edit then git commit/push
```

### Syncing all modules

This will sync everything in the `managed_modules.yml`.

```bash
bundle exec msync update --message "modulesync $(git describe)"
```

Now you can use [hub](https://github.com/github/hub) to create pull requests.

```bash
./bin/create-pull-requests
```

You can now also create pull requests with modulesync directly:

```bash
export GITHUB_TOKEN=token
bundle exec msync update --message "modulesync $(git describe)" --pr --pr-labels modulesync --pr-title "modulesync $(git describe)"
```

### Clean up old mess before syncing

```bash
./bin/clean-git-checkouts
```

### Get a list of open todos

We have a nice script to detect a bunch of maintenance jobs in our modules. For
example wrong puppet version constraints or missing support for new operating
systems:

```bash
bundle install --path --without development
export GITHUB_TOKEN=token
bundle exec bin/get_all_the_diffs
```

You can also pass `DEBUG=true` as an environment variable to te script. to get
a bit more output.
