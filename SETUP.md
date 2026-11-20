# 维护手册

**冷白实验室风 · 中文默认 + 英文可切。** 主页 = 本仓库 `README.md`，地址 `github.com/YimoLab`。

## 文件职责

| 文件 | 作用 |
|:--|:--|
| `README.md` | 中文首页（GitHub 只渲染这一个） |
| `README.en.md` | 英文版，从顶部语言条进入 |
| `assets/banner.svg` | 头图：量测栅格、规格表、标尺、条码、15s 扫描头 |
| `assets/divider.svg` | 分区发丝线 |
| `assets/social-preview.svg` | 社交分享卡片源文件（1280×640，导出 PNG 后在 Settings → Social preview 上传） |
| `assets/lang/` | 语言条四枚铁片：`label` `zh-on` `zh-off` `en-on` `en-off` |
| `.github/workflows/snake.yml` | 每晚生成贡献蛇 → 推到 `output` 分支 |
| `variants/console/` | 暗色控制台版整套存档（已 gitignore，本地回退用） |

## 改东西去哪改

| 想改 | 位置 | 注意 |
|:--|:--|:--|
| 头图标题 `YIMO LAB` | `banner.svg` 两处 `<text>`（辉光 + 实体） | 8~12 字符最佳 |
| 岗位 / 模块行 | `banner.svg` 的 `FULL-STACK ENGINEER…` 与 `MODULES…` 两行 | 整行 40 字符内 |
| 规格表四行 | `banner.svg` 的 `STACK / SECTOR / STATUS / BUILD` | 值列起点 x=896，别超 250px |
| 项目表 | 两份 README 的「代表项目」 | 外链统一带 `↗` 记号；`target` 会被 GitHub 剥掉 |
| 自我介绍 | 两份 README 的「我在做什么」+ JSON 块 | 键名保持英文 |
| 博客 / 邮箱 | 两份 README 底部导航行 | `mailto:` 不需要 `target` |
| 其他链接里的用户名 | 已全部是 `YimoLab` | 换账号时全局替换 |

> **唯一纪律**：中英两份都要改。没有编译期检查，只有你自己盯着。

## 上线

```powershell
git add -A
git commit -m "content: ..."
git push
```

- Actions 报 403 → `Settings → Actions → General → Workflow permissions` 选 **Read and write**，再跑一次。
- 换过头图主题色 / 语言开关配色 → 必须重跑一次 `Generate contribution snake`，蛇的配色写死在 workflow 里。

## 双语机制

- 只有 `README.md` 会渲染成主页；`README.en.md` 只能通过语言条进（打开的是 GitHub 文件页，宽度略窄，图片走同一套相对路径）。
- 语言条是**五枚等高 SVG**，切换只是换 `src`，零脚本。选中态/未选中态各一枚，所以不需要 CSS。
- 加第三语言：复制 `en-off.svg` 改成 `ja-off.svg`，复制一份 `README.en.md` 成 `README.ja.md`，语言条加一枚 `<a>`。

## 换主题

```powershell
# 切到暗色控制台
Copy-Item .\variants\console\README.md    .\README.md -Force
Copy-Item .\variants\console\README.en.md .\README.en.md -Force
Copy-Item .\variants\console\assets\*     .\assets\ -Recurse -Force
Copy-Item .\variants\console\.github\workflows\snake.yml .\.github\workflows\snake.yml -Force
```

回实验室：切换前先 commit，之后 `git checkout HEAD~1 -- .`。

## 已知坑

| 现象 | 原因 | 处理 |
|:--|:--|:--|
| 分隔线整条不见，只剩刻度 | 水平线用了 `objectBoundingBox` 渐变，包围盒高度为 0 → 浏览器不渲染 | 渐变加 `gradientUnits="userSpaceOnUse"` 并写死 `x1/x2/y1/y2` |
| 项目名被折成两行 | 窄列里连字符是合法断点 | 用不换行连字符 `&#8209;` |
| 想新标签页打开，但不生效 | GitHub 清洗器会删掉 `target` 和 `rel`（实测渲染成 `rel="nofollow"`） | 无解，改用链接文字后加 `↗` 提示外链 |
| 中文徽章变方块 | shields URL 没 percent-encode | `[uri]::EscapeDataString` 编码后再拼 |
| 改完 SVG 页面没变 | GitHub camo 缓存，最长 10 分钟 | 想立刻看：把 `assets/banner.svg` 直接拖进浏览器 |
| 蛇图 404 / 颜色不对 | Actions 没跑，或换主题后没重跑 | 检查 `output` 分支，重跑 workflow |

## 想换掉单色强调

全站只有一个强调色 `#0B63F6`（`banner.svg`、`divider.svg`、`assets/lang/*.svg`、README 卡片 URL 里的 `title_color` / `line` / `ring`）。
把它换成 `#0F766E`（医疗绿）或 `#B45309`（琥珀），结构不动、气质立刻换掉。
