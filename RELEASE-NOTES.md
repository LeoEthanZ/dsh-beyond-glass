# v0.1.0

First release: the three UI blocks, as six packages. See [README.md](README.md)
for the install command.

## Files

| File | What it is |
| --- | --- |
| `beyond-glass-harness-0.1.6-alpha.2.tgz` | The profile layer. Mounts the five client rows below, and carries the three things a clean official host cannot answer for itself: the dashboard's topic and weather feeds, and the launcher's cover assets. |
| `beyond-glass-ui-layout-0.1.5-rc.2.tgz` | The three-column frame. |
| `beyond-glass-ui-sidebar-0.1.5-rc.2.tgz` | The left column. |
| `beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz` | The right column. |
| `deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz` | The wallpaper picker, and the glass token sheet every panel reads its background and blur from. |
| `deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz` | The launcher: its shell rail, the sidebar seats it owns, and the dashboard. |

Install all six in one command — only the harness package is a profile layer;
the other five are mounted by the rows it patches.

## Compatibility

Built against `@deepseek-ai/dsh@0.1.5-rc.2`, verified on `0.1.6-alpha.2`.
Installing builds the `web` profile under `~/.dsh/profiles/web`; it leaves your
harness install alone, and removes cleanly.

## Known limits

- A harness release that adds a row occupying one of the seats this UI registers
  collides with it. The client then fails to load with a single-line
  `Failed to load plugins` and nothing else renders. The fix belongs here.
- The harness package is ~26 MB, most of it template preview assets the launcher
  and the built-in app cards display.

## License

MIT, with the original `deepseek-harness` copyright notice retained inside each
package.

---

# v0.1.0（中文）

首个版本：三块界面，六个包。安装命令见 [README.zh.md](README.zh.md)。

| 文件 | 是什么 |
| --- | --- |
| `beyond-glass-harness-0.1.6-alpha.2.tgz` | profile layer。挂载下面五个客户端行，并自带干净官方宿主无法提供的三样东西：仪表的话题与天气数据源、启动台的应用卡封面素材。 |
| `beyond-glass-ui-layout-0.1.5-rc.2.tgz` | 三栏框架。 |
| `beyond-glass-ui-sidebar-0.1.5-rc.2.tgz` | 左侧栏。 |
| `beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz` | 右侧栏。 |
| `deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz` | 壁纸选择器，以及所有面板的背景与模糊所依赖的玻璃令牌表。 |
| `deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz` | 启动台：外壳竖栏、它负责的侧栏座位，以及数据仪表。 |

六个包请一次装完 —— 只有 harness 那个是 profile layer，另外五个由它 patch 的行挂载。

**兼容性**：按 `@deepseek-ai/dsh@0.1.5-rc.2` 构建，`0.1.6-alpha.2` 上验证通过。安装会在
`~/.dsh/profiles/web` 下建 `web` profile，不影响你的 harness 本体，也能干净卸载。

**已知限制**：harness 版本若新增一行、恰好占用这套界面注册的座位，会撞车 —— 客户端以一行
`Failed to load plugins` 报错，其余不渲染，修的地方在这个发行包这边。harness 包约 26 MB，
大部分是启动台与内置应用卡展示的模板预览素材。

**许可**：MIT，每个包内保留 `deepseek-harness` 的原始版权声明。
