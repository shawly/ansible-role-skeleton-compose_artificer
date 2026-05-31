# shawly.compose_artificer_skeleton

Skeleton role for creating roles that use [`shawly.compose_artificer`](https://github.com/shawly/ansible-role-compose_artificer).

## Usage

To make creating new roles that use `shawly.compose_artificer` easier, use the
`shawly.compose_artificer_skeleton` role e.g.:

```bash
git clone https://github.com/shawly/ansible-compose_artificer_skeleton.git shawly.compose_artificer_skeleton
ansible-galaxy init --role-skeleton shawly.compose_artificer_skeleton "${role_name:?}"
```

[See example](https://github.com/shawly/ansible-role-compose_artificer#skeleton)

## Tags

The generated role exposes the following tags (replace `myrole` with your role name):

- `myrole` - deploy this service (set facts, migrate, install, run)
- `compose_artificer` - deploy every compose_artificer-based service at once
- `myrole_migrate` - migrate data from an old location
- `myrole_install` - create directories, snapshots and upload the compose files
- `myrole_run` - pull images and (re)start the stack
- `myrole_down` - `docker compose down` and prune old snapshots, keeping the latest
  `myrole_down_snapshots_keep` (default `1`); leaves data intact
- `myrole_destroy` - `docker compose down` and wipe the stack, all snapshots, datasets/subvolumes and project
  directories (destructive)

The `_down` and `_destroy` blocks are guarded with the special `never` tag, so they only run when their tag is passed
explicitly via `--tags`. Each phase has matching `pre_`/`post_` hook files in `tasks/` (e.g. `pre_down.yml`,
`post_destroy.yml`) that you can customize.

> **Important:** do **not** put tags at the *play* level when including this role. Ansible copies play-level tags onto
> every task in the play - including the `never`-guarded `_down`/`_destroy` blocks - which overrides `never` and lets a
> routine `--tags myrole` (or any category tag) trigger a teardown. Keep the play untagged and rely on the per-block
> tags above; deploy a category by running the relevant playbook instead.

## License

GPL-3.0-or-later

## Author Information

Created by [shawly](https://github.com/shawly).
