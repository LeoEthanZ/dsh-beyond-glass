# Beyond Glass

[deepseek-harness](https://github.com/deepseek-ai/deepseek-harness) 的三栏玻璃界面，
做成了**一套能装也能卸的包**。不用换 harness，也不动你的任何文件：六个包一起装进一个独立
profile，卸掉就恢复原样。

[English →](README.md)

**[在线展示页 →](https://leoethanz.github.io/dsh-beyond-glass/)** —— 九张壁纸在一个页面里
看完，点一下就换；中间那帧的周围是同一张壁纸，真的在动。打开页面时，窗口会自己放一段九秒的
录屏：收起一栏、换一张壁纸、打开数据仪表，放完交回静态截图；滚回页首，或者换过别的壁纸再切
回这一张，它还会再放一遍。

![三栏总览](docs/01-shell-three-columns.jpg)

- [三块界面](#三块界面)
- [设计](#设计)
- [安装](#安装)：[没装过 harness](#没装过-harness) ／ [已经装了 harness](#已经装了-harness)
- [包里有什么，没有什么](#包里有什么没有什么)
- [兼容性](#兼容性) · [许可](#许可)

## 三块界面

| 块 | 是什么 | 包 |
| --- | --- | --- |
| **玻璃外壳** | 三栏框架：品牌竖栏、左侧栏、右侧栏，都落在半透明玻璃面板上 | `ui-layout` `ui-sidebar` `ui-sidebar-right` |
| **启动台** | 首页中栏：类目网格、应用卡、以及沉浸模式下的数据仪表 | `ui-workbench` |
| **壁纸** | 右侧栏的壁纸选择器，以及所有面板共用的玻璃令牌表 | `ui-wallpaper` |

六个包里只有 `harness` 那个是 profile layer，其余五个挂在它 patch 出来的行上。

## 设计

### 三栏玻璃外壳

左栏是身份与导航（品牌字标、新会话／启动台、类目树、会话列表、API 账户），中栏是启动台，
右栏是当前工作面（会话、壁纸，以及其它插件注册进来的面板）。三栏共用同一套玻璃令牌：
半透明渐变底 + `backdrop-filter: blur(28px) saturate(1.8)` + 0.5px 描边。

左栏可以收成一条图标轨道，右栏可以整列折叠；壁纸透到三栏底下。

![三栏总览](docs/01-shell-three-columns.jpg)

右栏是「当前工作面」而不是固定的一栏：它可以是会话，可以是壁纸，也可以是别的插件注册进来的
面板（下图为插件市场）。

![右栏承载的其它面板](docs/03-right-column-plugin-market.jpg)

### 壁纸

「极境壁纸」面板提供两类：**程序化流体**四款（深海流光、极光翡翠、紫晶云海、熔金暮色），
和**动态自然风光**五段（雾隐森林、潮汐漫游、雪峰天际、雪岭电影感、雪野滑踪）。

流体是页面内绘制的，不联网；自然风光是视频素材，首次播放需要联网（面板内标注「需要联网」）。
素材来自 [Coverr](https://coverr.co/)，可商用、免署名。

![壁纸选择器](docs/04-wallpaper-picker.jpg)

<p>
  <img src="docs/02-wallpaper-fluid.jpg" width="32%" alt="另一种流体配色" />
  <img src="docs/05-wallpaper-nature.jpg" width="32%" alt="海浪" />
  <img src="docs/06-wallpaper-snow.jpg" width="32%" alt="雪岭" />
</p>

### 启动台与数据仪表

中栏按类目铺开应用卡（封面 + 悬停动效），带「编辑」「注册」入口和分页。
进入**沉浸模式**后，右半屏是数据仪表，四张卡：

| 卡 | 内容 |
| --- | --- |
| **AI 热点** | Top 10 标题与分类标签，随包提供数据源 |
| **TOKEN 用量** | 今日与历史累计 token，可切日／周／月，读你自己的本地用量 |
| **今日** | 日期 + 本地天气（城市、温度、高低、天况） |
| **待办** | 未完成待办，同步自右侧栏的待办面板 |

![沉浸模式下的数据仪表](docs/07-dashboard-immersive.jpg)

壁纸在沉浸模式下照样生效，仪表落在它上面：

![换一张壁纸后的同一套仪表](docs/08-dashboard-immersive-snow.jpg)

其中热点与天气由宿主侧取数，待办和右栏面板共用一份数据 —— 这三处的边界见
[包里有什么，没有什么](#包里有什么没有什么)。

## 安装

需要 Node.js、**pnpm** 和 `dsh` 命令（还没有 `dsh` 的话，下面的第一行命令会装上）。不需要
clone，不需要构建。`dsh plugin` 底下转发给的就是 pnpm，所以 pnpm 得先在 PATH 上 —— Windows
上装完 Node 默认没有 pnpm，这一步最容易漏。

六个包必须**一次装完**：只有 `harness` 那个是 profile layer，负责挂载另外五个。
分开装也能装上，只不过只有最后那个是 layer，其余会退化成普通依赖。

`BASE` 是同一份 Release 的下载地址。下面每个代码块里都自己设一遍，写的也是各自 shell 的语法，
就在用到它的命令前面 —— 所以每个块都是完整的，整块复制就能跑。

下面的命令都给了两份：`bash`/`zsh` 一份，PowerShell 一份。照你用的那个抄，两份不能混着用。

### 没装过 harness

**macOS / Linux —— `bash`、`zsh`**

```sh
npm install -g @deepseek-ai/dsh@0.1.5-rc.2

BASE=https://github.com/LeoEthanZ/dsh-beyond-glass/releases/download/v0.1.2

# 先把 profile 建出来（只用 add 建出来的 profile 没有界面层）
dsh --profile glass --from-default-profile web --dump-config > /dev/null

dsh plugin --profile glass add \
  $BASE/beyond-glass-harness-0.1.6-alpha.2.tgz \
  $BASE/beyond-glass-ui-layout-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz \
  $BASE/deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz \
  $BASE/deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz

dsh --profile glass
```

**Windows —— PowerShell**

```powershell
npm install -g @deepseek-ai/dsh@0.1.5-rc.2

$BASE = "https://github.com/LeoEthanZ/dsh-beyond-glass/releases/download/v0.1.2"

# 先把 profile 建出来（只用 add 建出来的 profile 没有界面层）
dsh --profile glass --from-default-profile web --dump-config > $null

dsh plugin --profile glass add `
  "$BASE/beyond-glass-harness-0.1.6-alpha.2.tgz" `
  "$BASE/beyond-glass-ui-layout-0.1.5-rc.2.tgz" `
  "$BASE/beyond-glass-ui-sidebar-0.1.5-rc.2.tgz" `
  "$BASE/beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz" `
  "$BASE/deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz" `
  "$BASE/deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz"

dsh --profile glass
```

PowerShell 里面的反引号是续行符，必须是那一行的最后一个字符 —— 后面多一个空格，命令就断了。
`cmd.exe` 里两份都跑不通，用 PowerShell。

`--from-default-profile web` 那一行把 `~/.dsh/profiles/glass`（Windows 上是
`%USERPROFILE%\.dsh\profiles\glass`）按官方 web 模板建出来，`add` 再把六个包装进去；
`glass` 只是 profile 名，可以换成别的。

### 已经装了 harness

先确认版本：

```sh
dsh --version      # 需要 0.1.5-rc.2 或 0.1.6-alpha.2
```

版本对得上就直接装 —— 和上一节完全一样，只是不必重装 harness：

**macOS / Linux —— `bash`、`zsh`**

```sh
BASE=https://github.com/LeoEthanZ/dsh-beyond-glass/releases/download/v0.1.2

# 先把 profile 建出来（只用 add 建出来的 profile 没有界面层）
dsh --profile glass --from-default-profile web --dump-config > /dev/null

dsh plugin --profile glass add \
  $BASE/beyond-glass-harness-0.1.6-alpha.2.tgz \
  $BASE/beyond-glass-ui-layout-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-0.1.5-rc.2.tgz \
  $BASE/beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz \
  $BASE/deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz \
  $BASE/deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz

dsh --profile glass
```

**Windows —— PowerShell**

```powershell
$BASE = "https://github.com/LeoEthanZ/dsh-beyond-glass/releases/download/v0.1.2"

# 先把 profile 建出来（只用 add 建出来的 profile 没有界面层）
dsh --profile glass --from-default-profile web --dump-config > $null

dsh plugin --profile glass add `
  "$BASE/beyond-glass-harness-0.1.6-alpha.2.tgz" `
  "$BASE/beyond-glass-ui-layout-0.1.5-rc.2.tgz" `
  "$BASE/beyond-glass-ui-sidebar-0.1.5-rc.2.tgz" `
  "$BASE/beyond-glass-ui-sidebar-right-0.1.5-rc.2.tgz" `
  "$BASE/deepseek-ai-dsh-client-ui-wallpaper-0.1.2-rc.1.tgz" `
  "$BASE/deepseek-ai-dsh-client-ui-workbench-0.1.2-rc.1.tgz"

dsh --profile glass
```

**这套界面装在一个独立 profile 里**，你原有的 profile（还是那套插件、那份设置）一个字都不动，
`dsh` 的启动方式照旧。想用它就用 `dsh --profile glass`，不想用就继续敲原来的命令。

三个坑：

- **别用你已经占用的 profile 名**。名字被占用时，`add` 是往你那个日常 profile 上叠，
  不是新建一个。挑一个没用过的名字（示例里是 `glass`）。
- **版本不符**（既不是 `0.1.5-rc.2` 也不是 `0.1.6-alpha.2`）时，这套界面没测过。
  钉到测过的版本再装：`npm install -g @deepseek-ai/dsh@0.1.5-rc.2`。
- **`dsh --profile glass` 敲下去什么都不打印**，说明这个 profile 是 `add` 单独建出来的、
  里面没有界面层。删掉 `~/.dsh/profiles/glass` 再照上面的块整段跑一遍就好 ——
  建 profile 那一行就是它能不能起来的关键。

### 卸载

**macOS / Linux —— `bash`、`zsh`**

```sh
dsh plugin --profile glass remove \
  @beyond-glass/harness @beyond-glass/ui-layout @beyond-glass/ui-sidebar \
  @beyond-glass/ui-sidebar-right \
  @deepseek-ai/dsh-client-ui-wallpaper @deepseek-ai/dsh-client-ui-workbench
```

**Windows —— PowerShell**

```powershell
dsh plugin --profile glass remove `
  @beyond-glass/harness @beyond-glass/ui-layout @beyond-glass/ui-sidebar `
  @beyond-glass/ui-sidebar-right `
  @deepseek-ai/dsh-client-ui-wallpaper @deepseek-ai/dsh-client-ui-workbench
```

`dsh --profile glass` 这一路就没了；不想要整个 profile，删掉 `~/.dsh/profiles/glass`
（Windows 上是 `%USERPROFILE%\.dsh\profiles\glass`）目录即可。

## 包里有什么，没有什么

### 在包内

- 三栏框架、左侧栏、右侧栏，以及它们共用的玻璃令牌表。
- 启动台：类目网格、应用卡的外观与动效、「编辑」「注册」入口、分页。
- 启动台**装完就有那九张卡**：名称、说明、封面与两个类目随包提供，装完就和截图里一样。
- 沉浸模式与数据仪表这四张卡的**外观与取数通路**。
- 壁纸选择器与两类壁纸（流体页面内绘制；自然风光为随包分发的视频素材）。
- 让上面这些在**干净官方宿主**上成立的三样宿主能力：热点与天气两个 feed，
  以及应用卡与模板预览的封面素材（否则就是破图，和永远停在空态的卡片）。

### 不在包内

**卡片背后的应用。** 那九张卡随包提供，但它们打开的应用不在包里：品牌 PPT 制作、音视频转文本、
员工信息管理、知识库阅读、项目管理、应用开发、小红书、UI 组件库，还有「注册」这个入口 ——
各自都是独立的插件。点开一张没接应用的卡，提示是「应用已注册但未接入」。本包给的是启动台这个
容器、卡片的呈现方式，以及它装完时的这份清单；你的启动台里能真正打开什么，取决于你自己装了什么。

**右栏里的工具面板。** 插件市场、颜色换肤、统一应用封面这类面板，都是各自的插件自己注册进右栏的。
本包给的是右栏本身，以及其中**会话**和**壁纸**这两块面板；其余的你装了才有。

**数据仪表的内容来源。** 四张卡不是同一种性质：

| 卡 | 你还需要做什么 |
| --- | --- |
| AI 热点 | 不用。随包提供数据源，装完即有内容 |
| TOKEN 用量 | 不用。读你自己的本地会话用量 |
| 今日 + 天气 | 宿主机需要能访问 `whois.pconline.com.cn`（按出口 IP 定位城市）与 `open-meteo.com`（取天气）。取不到时卡片停在「正在定位…」，不会报错 |
| 待办 | **需要自己接**。待办卡同步自右侧栏的待办面板，而那个面板与它的数据源不在本包内，所以这张卡在干净环境里是空的 |

天气卡的两处网络要求值得留意：它按**出口 IP**定位城市，并且是**宿主侧**发起请求。
如果你的机器走代理，定位拿到的可能不是你的城市，甚至取不到 —— 这时卡片保持空态。

## 兼容性

按 `@deepseek-ai/dsh@0.1.5-rc.2` 构建，在 `0.1.6-alpha.2` 上验证过。

如果某个 harness 版本新增了一行，恰好占了这套界面注册的位置，就会撞车：客户端会以一行
`Failed to load plugins` 报错，其余什么都不渲染。真碰上了，要改的是这个发行包，
不用动你的环境。

## 许可

MIT，与它所基于的 harness 一致。见 [LICENSE](LICENSE)：`deepseek-harness` 的原始版权声明
保留着，发行版的每个包内也各带一份。
