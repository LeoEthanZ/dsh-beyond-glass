# v0.1.2

Three fixes for what a stranger's install sees. Content update: the six packages
keep the versions they had in v0.1.1, so install from this tag rather than that
one — a `v0.1.2` URL supersedes `v0.1.1` outright.

## What changed

- **The nine launcher cards are one size now.** A card is a `<button>`, and a
  button is sized to its contents: with the description on a single nowrap line,
  a card whose description was long outgrew its track — 318px inside a 223.8px
  column — overlapping the card beside it, while the ellipsis never appeared.
  The cards now fill their cell, so the covers line up and long descriptions
  clip where they should.
- **Settings opens as a full-window sheet.** The sheet renders inside the left
  column, and that column carried the glass `backdrop-filter` — which makes an
  element the containing block for its own fixed-position descendants. The sheet
  was therefore held to the column's box: 264×884 inside a 1440×900 window. The
  refraction now rides a pseudo-element, so the column no longer captures it.
- **The skin is dark on a light machine.** Theme preference defaults to `system`,
  so a light-mode host turned the shell light and both side columns came out
  white. The skin now reads `system` as dark. Picking Light or Dark in Appearance
  still does exactly what it says.

Everything else is v0.1.1: the three-column glass shell, the wallpaper column,
the immersive dashboard, and the nine-card catalog.

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

- **The nine cards render at one width** — 223.8px each, one per grid track,
  zero overlapping pairs — and every cover measures 223.8×125.9 (16:9). The
  three long descriptions clip.
- **The settings sheet measures 1440×900 at 0,0** — full window, and the column
  it renders inside no longer carries a containing-block property.
- **On a host set to light mode the shell still renders dark** — `body` at
  `rgb(5,8,14)`, `color-scheme: dark`, with the same result on a dark host.
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

# v0.1.2（中文）

这一版修的是「换个干净环境装一遍才会看见的三个毛病」。内容更新：六个包的版本号与 v0.1.1 相同，
所以请从本 tag 安装 —— `v0.1.2` 的地址完全取代 `v0.1.1`。

## 改了什么

- **九张卡现在一样宽。** 应用卡本身是 `<button>`，按钮的宽度由内容定：说明文字是一行不折行的，
  说明一长，卡片就撑出自己的格子（223.8px 的轨道里长出 318px），压到旁边那张上，省略号也一直
  不生效。现在卡片撑满格子，封面排齐，过长的说明正常截断。
- **设置是整屏浮层，不再被左边那栏框住。** 设置面板渲染在左栏内部，而左栏带着玻璃那层
  `backdrop-filter` —— 元素一旦用上它，就成了自己内部 fixed 定位子元素的包含块，面板因此被限在
  左栏里：1440×900 的窗口里只有 264×884。现在折射效果挪到伪元素上，左栏不再参与定位。
- **系统是浅色，界面照样是深色。** 主题默认跟随系统，浅色机器上外壳就跟着变白，两列侧栏全白。
  现在这套皮肤一律把「跟随系统」当深色；外观里手动选浅色或深色，仍然选了就生效。

其余与 v0.1.1 相同：三栏玻璃外壳、壁纸栏、沉浸模式数据仪表，以及那九张卡的清单。

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
九张卡宽度一致（223.8px，各占一个轨道），没有互相压叠，九张封面都是 223.8×125.9（16:9），
三段过长的说明正常截断；设置浮层 1440×900、起点 0,0，整屏铺满，它所在的那一栏上再没有会兜住
它的属性；宿主系统设成浅色时界面仍是深色（`body` 为 `rgb(5,8,14)`，`color-scheme: dark`），
深色宿主下同样。上一版验过的四项也复算过：启动台渲染 9 张卡，名称与封面 preset 与清单一致，
九张封面全部 `200 image/webp`；`/trending`、`/weather`、`/workbench/covers/*.webp` 均 `200`，
伪造路径 `404`；只读模式下玻璃令牌有值（两列侧栏都是 `blur(28px) saturate(1.8)` + 渐变 +
发丝描边）；仪表四个卡位全部落到真值，无破图。

**已知限制**：harness 版本若新增一行、恰好占了这套界面注册的位置，会撞车 —— 客户端以一行
`Failed to load plugins` 报错，其余不渲染，要改的是这个发行包。harness 包约 26 MB，
大部分是启动台与内置应用卡展示的模板预览素材。今日天气卡依赖宿主机能访问
`whois.pconline.com.cn` 与 `open-meteo.com`，取不到时卡片停在空态且日志无痕。

**许可**：MIT，每个包内保留 `deepseek-harness` 的原始版权声明。
