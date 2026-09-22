# Beyond Glass

[deepseek-harness](https://github.com/deepseek-ai/deepseek-harness) 的三栏玻璃界面，
以**可安装的包**形式发布，不是一个 fork。

[English →](README.md)

三块内容：

- **玻璃外壳** —— 三栏框架：品牌竖栏、左侧栏、右侧栏，都落在半透明面板上。
- **启动台** —— 首页中栏，含数据仪表：AI 热点、Token 用量、天气、待办。
- **壁纸** —— 右侧栏的壁纸选择器。

## 安装

需要 Node.js、pnpm 和 `dsh` 命令，不需要 clone，也不需要构建。

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

第一条命令会在 `~/.dsh/profiles/web` 下建好 `web` profile（若不存在）。六个包要
**一次装完**：其中一个是 profile layer，负责挂载另外五个；分开装也行，但那样只有
最后那个是 layer，其余会退化成普通依赖。

卸载：

```sh
dsh plugin --profile web remove beyond-glass-harness beyond-glass-ui-layout \
  beyond-glass-ui-sidebar beyond-glass-ui-sidebar-right \
  deepseek-ai-dsh-client-ui-wallpaper deepseek-ai-dsh-client-ui-workbench
```

## 它不做什么

- **不是 fork，也不改你的任何文件。** 六个包以 profile layer 的形式挂载，你的 `dsh`
  本体不受影响；卸掉就回到原样。
- **不是官方产物。** 这是官方 harness 上的第三方界面皮肤。包名里那几个 `deepseek-ai`
  前缀只是为了配合 loader 按包名解析行，与官方作者无关。
- **只画界面。** 不含 agent 行为、不含模型配置、不自带数据存储。

## 兼容性

按 `@deepseek-ai/dsh@0.1.5-rc.2` 构建，在 `0.1.6-alpha.2` 上也验证过。如果某个 harness
版本新增了一行，恰好占用这套界面注册的座位，就会撞车：客户端会以一行
`Failed to load plugins` 报错，其余什么都不渲染。真发生时，修的地方在这个发行包这边，
不是你的环境。

## 许可

MIT，与它所基于的 harness 一致。见 [LICENSE](LICENSE)：`deepseek-harness` 的原始版权
声明予以保留，发行版中每个包内也各带一份。
