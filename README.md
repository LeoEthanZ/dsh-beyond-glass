# Beyond Glass

A three-column glass UI for [deepseek-harness](https://github.com/deepseek-ai/deepseek-harness),
delivered as installable packages rather than as a fork.

[中文说明 →](README.zh.md)

Three blocks:

- **Glass shell** — the three-column frame: brand rail, sidebar, right column,
  all on translucent panels.
- **Launcher** — the centre panel of the home view, with its dashboard: trending
  topics, token usage, weather, and todos.
- **Wallpaper** — the wallpaper picker in the right column.

## Install

You need Node.js, pnpm, and the `dsh` CLI. Nothing else — no clone, no build.

```sh
npm install -g @deepseek-ai/dsh@0.1.5-rc.2

BASE=https://github.com/LeoEthanZ/dsh-beyond-glass/releases/download/v0.1.0
dsh plugin --profile web add \
  $BASE/beyond-glass-harness-0.1.6-alpha.2.tgz \
  $BASE/beyond-glass-ui-layout-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz \
  $BASE/deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz \
  $BASE/deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz

dsh --profile web
```

The first command creates the `web` profile under `~/.dsh/profiles/web` if it
does not exist. All six packages go in one `add`, because one of them is the
profile layer that mounts the other five; installing them one at a time works
too, but only the last one is a layer and the rest are plain dependencies.

To remove:

```sh
dsh plugin --profile web remove beyond-glass-harness beyond-glass-ui-layout \
  beyond-glass-ui-sidebar beyond-glass-ui-sidebar-right \
  deepseek-ai-dsh-client-ui-wallpaper deepseek-ai-dsh-client-ui-workbench
```

## What it does not do

- **It is not a fork and it does not patch anything.** The packages mount as
  profile layers; your `dsh` install is untouched, and removing them puts you
  back where you started.
- **It is not official.** It is a third-party UI over the official harness. The
  package names carrying a `deepseek-ai` prefix are kept only because the
  loader resolves rows by package name.
- **It only draws UI.** No agent behaviour, no model configuration, no data
  storage of its own.

## Compatibility

Built against `@deepseek-ai/dsh@0.1.5-rc.2`, verified on `0.1.6-alpha.2` as well.
A harness release that adds a row occupying one of the seats this UI registers
would collide with it: the client then fails to load with a single-line
`Failed to load plugins` message and nothing else renders. If that happens, the
fix lives in this distribution, not in your setup.

## License

MIT, as the harness it is built on. See [LICENSE](LICENSE) — the original
copyright notice from `deepseek-harness` is retained, and each package in the
release carries a copy.
