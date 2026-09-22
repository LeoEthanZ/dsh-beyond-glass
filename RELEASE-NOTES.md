# v0.1.1

The launcher now opens on a populated 启动台. Content update: the six packages
keep the versions they had in v0.1.0, so install from this tag rather than that
one — a `v0.1.1` URL supersedes `v0.1.0` outright.

## What changed

- **The launcher carries its own first-run contents.** A clean install used to
  open on two cards — the only two that really ship an application renderer. It
  now opens on the same nine cards the screenshots show, with their names,
  descriptions, cover art and both categories. The catalog is injected at build
  time into the launcher's compiled payload, from `launcher-catalog.json` beside
  the builder, so the fork's own defaults stay untouched.
- **The applications behind those cards are still not in the packages.** The nine
  cards are a catalog, not software: opening one that has no application behind
  it answers 「应用已注册但未接入」. The launcher is the container.

Everything else is v0.1.0: the three-column glass shell, the wallpaper column,
the immersive dashboard.

## Files

| File | What it is |
| --- | --- |
| `beyond-glass-harness-0.1.6-alpha.2.tgz` | The profile layer. Mounts the five client rows below, and carries the three things a clean official host cannot answer for itself: the dashboard's topic and weather feeds, and the launcher's cover assets. |
| `beyond-glass-ui-layout-0.1.5-rc.2.tgz` | The three-column frame. |
| `beyond-glass-ui-sidebar-0.1.5-rc.2.tgz` | The left column. |
| `beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz` | The right column. |
| `deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz` | The wallpaper picker, and the glass token sheet every panel reads its background and blur from. |
| `deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz` | The launcher: its shell rail, the sidebar seats it owns, its catalog, and the dashboard. |

Install all six in one command — only the harness package is a profile layer;
the other five are mounted by the rows it patches.

## Compatibility

Built against `@deepseek-ai/dsh@0.1.5-rc.2`, verified on `0.1.5-rc.2` (npm
`latest`). Installing builds the `web` profile under `~/.dsh/profiles/web`; it
leaves your harness install alone, and removes cleanly.

## Verified on a clean install

Six tarballs into a fresh `DSH_HOME` over the published `@deepseek-ai/dsh@latest`:

- 9 cards rendered, names and cover presets matching the catalog file, all nine
  covers served `200 image/webp`, no failed requests.
- `/trending` `200`, `/weather` `200`, `/workbench/covers/*.webp` `200`, and a
  made-up path under `/workbench/` `404`.
- Glass tokens resolve in read-only mode (`blur(28px) saturate(1.8)` plus the
  gradient and hairline on both side columns) — the packages carry their own
  token sheet.
- The dashboard's four slots all land values, with no broken image.

## Known limits

- A harness release that adds a row occupying one of the seats this UI registers
  collides with it. The client then fails to load with a single-line
  `Failed to load plugins` and nothing else renders. The fix belongs here.
- The harness package is ~26 MB, most of it template preview assets the launcher
  and the built-in app cards display.
- The 今日 weather card depends on the host reaching `whois.pconline.com.cn` and
  `open-meteo.com`. When it cannot, the card keeps its empty state and logs
  nothing.

## License

MIT, with the original `deepseek-harness` copyright notice retained inside each
package.

---

# v0.1.1（中文）

启动台现在装完就是满的。内容更新：六个包的版本号与 v0.1.0 相同，所以请从本 tag 安装 ——
`v0.1.1` 的地址完全取代 `v0.1.0`。

## 改了什么

- **启动台自带首屏内容。** 以前干净安装打开是两张卡（只有那两个真带应用渲染器）；现在打开
  就是截图里那九张，名称、说明、封面与两个类目都随包提供。清单在打包时注入启动台的编译产物，
  数据来自紧挨着打包器的 `launcher-catalog.json`，fork 自己的默认值不动。
- **卡片背后的应用仍然不在包里。** 那九张卡是一份清单，不是软件：点开一张没有应用接入的卡，
  提示是「应用已注册但未接入」。本包给的是启动台这个容器。

其余与 v0.1.0 相同：三栏玻璃外壳、壁纸栏、沉浸模式数据仪表。

## 六个包

| 文件 | 是什么 |
| --- | --- |
| `beyond-glass-harness-0.1.6-alpha.2.tgz` | profile layer。挂载下面五个客户端行，并自带干净官方宿主无法提供的三样东西：仪表的话题与天气数据源、启动台的应用卡封面素材。 |
| `beyond-glass-ui-layout-0.1.5-rc.2.tgz` | 三栏框架。 |
| `beyond-glass-ui-sidebar-0.1.5-rc.2.tgz` | 左侧栏。 |
| `beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz` | 右侧栏。 |
| `deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz` | 壁纸选择器，以及所有面板的背景与模糊所依赖的玻璃令牌表。 |
| `deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz` | 启动台：外壳竖栏、它负责的那几栏侧栏、装完时的清单，以及数据仪表。 |

六个包请一次装完 —— 只有 harness 那个是 profile layer，另外五个挂在它 patch 出来的行上。

**兼容性**：按 `@deepseek-ai/dsh@0.1.5-rc.2` 构建，在 `0.1.5-rc.2`（npm 的 `latest`）上验证通过。
安装会在 `~/.dsh/profiles/web` 下建 `web` profile，不影响你的 harness 本体，也能干净卸载。

**干净安装上的实测**：全新 `DSH_HOME` + npm `latest` 的官方包，装六个 tarball 后 —— 启动台渲染
9 张卡，名称与封面 preset 与清单文件一致，九张封面全部 `200 image/webp`、无失败请求；
`/trending`、`/weather`、`/workbench/covers/*.webp` 均 `200`，伪造路径 `404`；只读模式下玻璃令牌
有值（两列侧栏都是 `blur(28px) saturate(1.8)` + 渐变 + 发丝描边）；仪表四个卡位全部落到真值，
无破图。

**已知限制**：harness 版本若新增一行、恰好占了这套界面注册的位置，会撞车 —— 客户端以一行
`Failed to load plugins` 报错，其余不渲染，要改的是这个发行包。harness 包约 26 MB，
大部分是启动台与内置应用卡展示的模板预览素材。今日天气卡依赖宿主机能访问
`whois.pconline.com.cn` 与 `open-meteo.com`，取不到时卡片停在空态且日志无痕。

**许可**：MIT，每个包内保留 `deepseek-harness` 的原始版权声明。
