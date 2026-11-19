# GitHub 首页部署说明

当前生效：**冷白实验室 · 规格书面板**
世界观：一页测量纸——量测栅格、印刷套准十字、规格表、标尺、样本条码、扫描头。
配色只有两个角色：纸面（`#FBFDFF → #E7EFF7`）+ 单一强调色 `#0B63F6` + 三级蓝灰字（`#0A1A2B` / `#334E68` / `#8AA3B8`）。

```
github-profile/                    ← 推这个目录里的内容到仓库根
├── README.md                      ← 默认中文版（主页渲染的就是它）
├── README.en.md                   ← 英文版，从顶部语言条进入
├── assets/
│   ├── banner.svg                 ← 实验室风头图（栅格 / 规格表 / 标尺 / 扫描头 15s）
│   ├── divider.svg                ← 蓝色发丝分隔线
│   └── lang/                      ← 语言开关：zh-on / zh-off / en-on / en-off
├── .github/workflows/snake.yml    ← 每晚生成贡献蛇（浅色配色）
├── .gitignore                     ← 把本地文件挡在仓库外
├── SETUP.md                       ← 本文件
└── variants/console/              ← 暗色控制台版归档（已 gitignore，本地回退用）
```

> 推 `README.md`、`README.en.md`、`assets/`、`.github/` 这四样就够主页跑起来。
> `variants/` 和 `*.png` 已写进 `.gitignore`，不会进仓库。

## 五步上线

1. **建仓库**：新建公开仓库，名字必须和用户名**完全一致** → `YimoLab`。它的 `README.md` 就是 `github.com/YimoLab` 的主页。
2. **传文件**：把 `README.md`、`README.en.md`、`assets/`（含 `lang/`）、`.github/` 推到仓库根目录。
   `.github/workflows/snake.yml` 必须保持这个路径，否则 Actions 不认。
3. **换文案**：用户名 `YimoLab` 已填好（README 里 11 处），剩下的岗位、模块标签、项目按下面表格改；**中英两份要同步**。
4. **开 Actions**：`Settings → Actions → General → Workflow permissions` 选 **Read and write permissions**，
   然后 `Actions → Generate contribution snake → Run workflow` 跑一次，`output` 分支出现后蛇图才会显示。
5. **验视**：刷新主页。若头图裂图，多半是相对路径写错（应为 `./assets/banner.svg`）。

## 已填 / 待改清单（实验室版）

| 项 | 当前值 | 状态 |
|:--|:--|:--|
| 仓库名 | `YimoLab` | ✅ 已定，必须与用户名一致 |
| GitHub 用户名 | `YimoLab` | ✅ 已填：统计卡、连击、活动图、关注数、访客数、snake 分支全部指向它 |
| 头图主标题 | `YIMO LAB` | ✅ 已填（`banner.svg` 里两处 `<text>`，建议 8~12 字符） |
| 岗位副标 | `FULL-STACK ENGINEER · SYSTEMS · INTERFACES` | ⬜ 待改，整行控制在 40 字符内 |
| 模块行 | `MODULES  TS · GO · RUST · K8S · WEBGL` | ⬜ 待改，值部分 ≤ 26 字符 |
| 规格表 STACK | `TS · GO · RUST` | ⬜ 待改 |
| 规格表 SECTOR | `SYSTEMS / INTERFACES` | ⬜ 待改 |
| 规格表 STATUS | `OPEN TO COLLAB` | ⬜ 待改（右侧小方块会呼吸） |
| 规格表 BUILD | `2026.11` | 装饰用，可删 |
| 标尺数字 | `0 / 600 / 1200` | 装饰用，可删 |
| 项目 | `PROJECT_ONE / TWO / THREE` | ⬜ 待改（链接文字 + URL 两处） |
| 自我介绍 | `我在做什么` 段落 + JSON 块 | ⬜ 待改 |

> 改完 `banner.svg` 后 GitHub 有 CDN 缓存，最长约 10 分钟才更新。
> 想立刻看效果：直接把 `assets/banner.svg` 拖进浏览器（SVG 自带动画，双击就能看），不用等 GitHub。

## 双语 / 国际化

GitHub 主页是**静态 Markdown，没有运行时语言切换**——能"切"的只有跳转。所以用文件互链：

| 文件 | 作用 |
|:--|:--|
| `README.md` | 默认语言（中文）。主页渲染的就是它 |
| `README.en.md` | 英文版，从顶部语言条跳过去 |
| `assets/lang/*.svg` | 4 枚语言开关（中/英 × 选中/未选中），手写 SVG，与头图同色 |

```html
<p align="center">
  <sub><code>LANG</code></sub>&nbsp;
  <a href="./README.md"><img src="./assets/lang/zh-on.svg" height="26" alt="中文" /></a><a href="./README.en.md"><img src="./assets/lang/en-off.svg" height="26" alt="English" /></a>
</p>
```

要点：

- **只有 `README.md` 会渲染成主页**，`README.en.md` 只能通过链接进入——点 EN 打开 GitHub 文件页，宽度略窄，图片走同一套相对路径，显示正常。
- 选中态/未选中态是**两枚独立 SVG**，切语言只是换 `src`，不需要任何脚本。
- 徽章文案跟着翻：中文版把文字 percent-encode 进 shields URL（`badge/%E7%8A%B6%E6%80%81-%E5%8F%AF%E5%90%88%E4%BD%9C-0B63F6`），英文版用英文。**改徽章时两份都要改**。
- 打字机文案按语言分开，中文版是编码后的中文长句；服务端返回的是真 SVG 文本，中文由访客端字体渲染，不会出现方块。
- 加第三语言：复制 `assets/lang/en-off.svg` 改文案成 `ja-off.svg`，再复制一份 README，语言条里加一枚 `<a>`。

## 切回暗色控制台 / 再切回来

```powershell
# 切到暗色控制台
Copy-Item .\variants\console\README.md    .\README.md -Force
Copy-Item .\variants\console\README.en.md .\README.en.md -Force
Copy-Item .\variants\console\assets\*     .\assets\ -Recurse -Force
Copy-Item .\variants\console\.github\workflows\snake.yml .\.github\workflows\snake.yml -Force
```

回到实验室：切换前先提交一次，之后 `git checkout` 回来即可（归档只有一份，不做双向副本）：

```bash
git add -A && git commit -m "switch: console"     # 切暗色前
git checkout HEAD~1 -- .                          # 想回来时
```

⚠️ 每次换主题后必须**重跑一次 snake action**——蛇的配色写死在 workflow 里，`output` 分支上的旧图不会自己变色。

## 换配色（一处改，全站统一）

`banner.svg` / `divider.svg` / `assets/lang/*.svg` 里的色值做全局替换，同时同步 README 卡片 URL 的
`bg_color` / `title_color` / `text_color`：

| 角色 | 当前值（实验室） | 说明 |
|:--|:--|:--|
| 纸面 | `#FBFDFF` `#F2F7FB` `#E7EFF7` | `paper` 渐变三档，一起改 |
| 墨色 | `#0A1A2B` | 名字、表头 |
| 正文蓝灰 | `#334E68` | 规格表数值 |
| 弱化 | `#8AA3B8` | 标签、标尺数字 |
| 结构线 | `#C3D3E0` | 规格表分隔线、开关描边 |
| 唯一强调 | `#0B63F6` | 蓝条、状态块、栅格、标尺 |

想变成别的单色强调（去掉"实验室蓝"）：把 `#0B63F6` 全局换成 `#0F766E`（医疗绿）或 `#B45309`（琥珀），
其余不动，整页气质立刻换掉而结构不塌。

## 可选增强（按需加，不要全加）

- **3D 贡献图**：`yoshi389111/github-profile-3d-contrib`，比蛇更抓眼，但和浅色纸面要重新调色。
- **自动指标卡**：`lowlighter/metrics`，首次配置约 20 分钟。
- **WakaTime 工时**：`athul/waka-readme`，需要把 Key 存到 Secrets。
- **自带实例反限额**：`github-readme-stats` 公共实例常 503，fork 到自己的 Vercel 换掉域名。

## 排错

| 现象 | 原因 | 处理 |
|:--|:--|:--|
| 分隔线整条不见，只剩两端刻度 | 水平线用了 `objectBoundingBox` 渐变，包围盒高度为 0，浏览器直接不渲染 | 渐变加 `gradientUnits="userSpaceOnUse"` 并显式写 `x1/x2/y1/y2`（已修） |
| 头图灰底裂图 | SVG 里引了外部字体或外链资源 | 只允许内联；`<image href>`、`@import` 都会被 camo 拦 |
| 动画不动 | 浏览器开了"减少动态效果"，或老 Safari | 结构本身可读，动画只是加分；需要的话把 SMIL 换成 CSS keyframes |
| 统计卡空白 | 公共实例限流 | fork 自建或换镜像 |
| 蛇图 404 / 颜色不对 | Actions 没跑，或换了主题没重跑 | 检查 `output` 分支，重跑 workflow |
| 中文徽章变方块 | shields URL 没做 percent-encode | 用 `[uri]::EscapeDataString` 编码后再拼 |
