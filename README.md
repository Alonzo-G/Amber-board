# Amber Board

单文件、零外部依赖的无限画布白板（个人版）。深色顶栏 + 白色画布 + 浅灰网格，直接用浏览器打开 `index.html` 即可使用。

## 功能

- **无限画布** — 空格/中键/平移工具拖拽，滚轮以光标为锚点缩放（0.12x–5x）
- **便签** — 点击添加、拖拽移动、双击行内编辑、右键菜单、6 色切换
- **画笔** — 二次贝塞尔平滑笔迹，6 色 + 1–24 粗细
- **连线** — 便签之间拖拽生成带箭头连线，按矩形边界求交定位锚点
- **选择** — 单选 8 个缩放手柄、多选联合外框、框选支持 Shift 追加
- **对齐吸附** — 拖拽时与邻近元素边/中线吸附并显示辅助线，按住 Alt 临时关闭
- **方向键微调** — 4 单位步进（Shift 为 24），连续按合并为一次撤销
- **迷你地图** — 右下角实时缩略图，可点击/拖拽导航
- **数据** — localStorage 自动保存、60 步撤销重做、导出/导入 JSON、导出 PNG

## 快捷键

| 操作 | 快捷键 |
|---|---|
| 选择 / 便签 / 画笔 / 连线 / 平移 | `V` `N` `P` `L` `H` |
| 平移画布 | 空格拖拽 / 中键拖拽 |
| 撤销 / 重做 | `Ctrl+Z` / `Ctrl+Shift+Z` |
| 全选 / 复制 | `Ctrl+A` / `Ctrl+D` |
| 适应内容 / 重置视图 | `Ctrl+1` / `Ctrl+0` |
| 删除选中 | `Delete` |
| 微调位置 | 方向键（`Shift` 大步进） |
| 提交编辑 / 取消编辑 | `Ctrl+Enter` / `Esc` |

## 说明

整站在单个 `index.html` 内：HTML/CSS/JS 全内联，无构建步骤、无网络请求。网格、笔迹、连线、便签、选中手柄统一绘制在同一块 Canvas 上（编辑文字时才用覆盖层 textarea），数据在浏览器本地，不上传服务器。

## 部署到 Cloudflare Pages

仓库根目录即为产物目录，**无需构建**。

1. Cloudflare 控制台 → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
2. 选中本仓库，构建设置：

   | 配置项 | 值 |
   |---|---|
   | Framework preset | `None` |
   | Build command | 留空 |
   | Build output directory | `/` |

3. 保存并部署，得到 `https://<项目名>.pages.dev`

仓库内的 `_headers` / `_redirects` 会被 Pages 自动识别：

- `_headers` — 严格 CSP（`connect-src 'none'`，禁止一切外连）、`nosniff`、禁止 iframe 嵌套、`no-cache` 保证更新即时生效
- `_redirects` — 旧文件名 `/my-board.html` 永久重定向到根地址

### 数据说明

白板内容存在浏览器 `localStorage`，**按域名隔离**：换域名、换浏览器、换设备都看不到原来的画板，首次访问会给出提示。跨环境迁移请用顶栏的 **导出 JSON → 导入 JSON**。

### 本地预览

```bash
python -m http.server 8000
# 打开 http://127.0.0.1:8000
```

直接双击 `index.html` 也可以，但此时 `_headers` 里的安全头不生效。
