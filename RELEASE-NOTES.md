# v0.1.3

One fix on top of v0.1.2. Content update: the six packages keep the versions
they had in v0.1.2, so install from this tag rather than that one — a `v0.1.3`
URL supersedes `v0.1.2` outright.

## What changed

- **The settings sheet answers clicks again.** It renders inside the left
  column, and the glass refracted layer makes that column a stacking context —
  so the sheet, a fixed-position descendant, was painted at the column's own
  level. With the launcher behind it the cards sat on top: measured on a live
  profile, the hit test at the sheet's centre returned an app card, and an
  unforced click on a nav cell timed out after 3s. The column now takes a level
  above the frame's own ladder (`handle` 11, `overlayLayer` 20) while a dialog
  is in it, so the sheet clears every column. With no dialog open the rule does
  not match and the closed state stays pixel-identical.

Everything else is v0.1.2: one card width, the full-window settings sheet, dark
on a light machine, and v0.1.1's three-column glass shell, wallpaper column,
immersive dashboard, and nine-card catalog.

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
the other five are mounted by the rows it patches. That command needs Node.js,
**pnpm** and the `dsh` CLI in place first, because `dsh plugin` forwards to
pnpm: with no pnpm on `PATH` it exits 127, and a fresh Node install on Windows
does not bring pnpm. Both spellings of the command — bash/zsh and PowerShell —
are in the [README](https://github.com/LeoEthanZ/dsh-beyond-glass#install).

## Compatibility

Built against `@deepseek-ai/dsh@0.1.5-rc.2`, verified on `0.1.5-rc.2` (npm
`latest`). Installing builds a profile of its own — `glass` in the examples,
under `~/.dsh/profiles/glass` — which leaves your harness install alone and
removes cleanly.

## Verified on a clean install

Six tarballs, fetched by their public URLs into a fresh `DSH_HOME` over the
published `@deepseek-ai/dsh@latest`:

- **The settings sheet takes a click** — the overlay measures 1440×900 at 0,0
  over a 1440×900 viewport, the hit test at its centre returns an element inside
  the sheet, and with the launcher behind it a real click on another nav cell
  moves the selection (0 → 1 of 4).
- **The nine cards render at one width** — 223.8px each, one per grid track,
  zero overlapping pairs — and every cover measures 223.8×125.9 (16:9). The
  three long descriptions clip.
- **On a host set to light mode the shell still renders dark** — `body` at
  `rgb(5,8,14)`, `color-scheme: dark`, with the same result on a dark host. The
  column carries no `backdrop-filter` of its own; the refraction's
  `blur(28px) saturate(1.8)` and gradient ride the layer behind it.
- `/trending` `200`, `/weather` `200`, `/workbench/covers/*.webp` `200`, and a
  made-up path under `/workbench/` `404`.
- 9 cards rendered, names and cover presets matching the catalog file, all nine
  covers served `200 image/webp`, no failed requests — each loading at its
  natural 960×540.
- The dashboard's four slots all render — 热点 with live headlines, TOKEN 用量
  with a count, 今日 with a real city and temperature, 待办 with its empty state.

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

# v0.1.3（中文）

这一版只有一处改动，叠在 v0.1.2 上。六个包的版本号跟 v0.1.2 一样，所以请从本 tag 安装 ——
`v0.1.3` 的地址完全取代 `v0.1.2`。

## 改了什么

- **设置面板又能点了。** 设置浮层渲染在左栏内部，而玻璃那层折射让左栏成了层叠上下文，浮层作为它的
  fixed 子元素，就跟着停在「左栏那一层」。启动台在背后时，应用卡压在浮层上：实机读数是，浮层中心
  命中的是应用卡，对导航按钮做一次不加 force 的真实点击，3 秒超时。现在左栏在持有浮层时抬到框架
  自己的层级阶梯之上（`handle` 是 11，`overlayLayer` 是 20），浮层就压得住每一列；没有浮层时这条
  规则不匹配，关闭态逐像素与之前一致。

其余与 v0.1.2 相同：卡片一样宽、设置整屏、浅色系统下界面仍是深色，以及 v0.1.1 的三栏玻璃外壳、
壁纸栏、沉浸模式仪表和那九张卡。

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
这条命令需要先装好 Node.js、**pnpm** 和 `dsh`：`dsh plugin` 底下转发给的就是 pnpm，`PATH`
上没有它，命令会以退出码 127 失败，而 Windows 上装完 Node 默认就是没有 pnpm。两条命令
（`bash`/`zsh` 和 PowerShell）都在
[README](https://github.com/LeoEthanZ/dsh-beyond-glass#install) 里。

**兼容性**：按 `@deepseek-ai/dsh@0.1.5-rc.2` 构建，在 `0.1.5-rc.2`（npm 的 `latest`）上验证通过。
安装会建它自己的 profile —— 示例里叫 `glass`，落在 `~/.dsh/profiles/glass` —— 不影响你的
harness 本体，也能干净卸载。

**干净安装上的实测**：六个 tarball 走公网地址装进全新 `DSH_HOME`，宿主是 npm `latest` 的官方包 ——
设置浮层点得动：整屏 1440×900、起点 0,0，浮层中心命中的是浮层内部的元素，启动台就在背后时点另一格
导航也会真的切换（选中格 0 → 1，共 4 格）；九张卡宽度一致（223.8px，各占一个轨道），没有互相压叠，
九张封面都是 223.8×125.9（16:9），三段过长的说明正常截断；宿主系统设成浅色时界面仍是深色（`body` 为
`rgb(5,8,14)`，`color-scheme: dark`），深色宿主下同样，左栏自己不背 `backdrop-filter`，折射的
`blur(28px) saturate(1.8)` 与渐变在它后面那层上；`/trending`、`/weather`、
`/workbench/covers/*.webp` 均 `200`，伪造路径 `404`；启动台渲染 9 张卡，名称与封面 preset 与
清单一致，九张封面全部 `200 image/webp`、以原尺寸 960×540 加载；仪表四个卡位全部正常渲染
（热点是真标题、TOKEN 用量有数字、今日是真城市与温度、待办显示空态）。

**已知限制**：harness 版本若新增一行、恰好占了这套界面注册的位置，会撞车 —— 客户端以一行
`Failed to load plugins` 报错，其余不渲染，要改的是这个发行包。harness 包约 26 MB，
大部分是启动台与内置应用卡展示的模板预览素材。今日天气卡依赖宿主机能访问
`whois.pconline.com.cn` 与 `open-meteo.com`，取不到时卡片停在空态且日志无痕。

**许可**：MIT，每个包内保留 `deepseek-harness` 的原始版权声明。
