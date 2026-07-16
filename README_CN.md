<p align="center">
  <img alt="mu-pdf-converter" src="assets/default-banner.png" width="100%">
</p>

# 🔄 mu-pdf-converter

> 高保真 PDF 格式转换工具 — 四层叠加解析、三引擎表格提取、外文自动翻译，将 PDF 转为完全可编辑的 PowerPoint、Word、Excel 和图片。

[English](README.md) | **中文** | [🌐 在线主页](https://mupoet.github.io/mu-pdf-converter/)

[![微信公众号](https://img.shields.io/badge/muippt-07C160?logo=wechat&logoColor=white)](https://mp.weixin.qq.com/s/v1JSZvlN5fvbOOHvkvXEtA)
[![小红书](https://img.shields.io/badge/muippt-FF2442?logo=xiaohongshu&logoColor=white)](https://xhslink.com/m/ESxtgUNMdl)
[![书籍](https://img.shields.io/badge/书籍-图解团队管理-BBDDE5?logo=bookstack&logoColor=white)](https://item.m.jd.com/product/14547345.html)
[![License](https://img.shields.io/github/license/mupoet/mu-pdf-converter)](LICENSE)
[![Version](https://img.shields.io/github/v/release/mupoet/mu-pdf-converter)](https://github.com/mupoet/mu-pdf-converter/releases)
[![Stars](https://img.shields.io/github/stars/mupoet/mu-pdf-converter)](https://github.com/mupoet/mu-pdf-converter/stargazers)

---

### 💡 使用场景示例

- 📊 **财务报表** — "把这份英文年报PDF转成PPT，自动翻译成中文"
- 📋 **批量发票** — "扫描整个invoices目录，把所有PDF里的表格提取到一个Excel"
- ✏️ **合同编辑** — "把这份PDF合同转成Word，我需要修改几个条款"
- 🧹 **去水印** — "这份PDF有水印影响阅读，帮我去掉"
- 📝 **表单填写** — "帮我填写这份PDF申请表，姓名填xxx，日期填xxx"
- 🖼️ **存档图片** — "把这份20页PDF每页转成高清PNG，300DPI"
- 🌍 **论文翻译** — "这份英文论文PDF转成PPT，要保留原始排版同时翻译"
- 📑 **数据提取** — "这个PDF报告第3-5页有几个表格，提取到Excel"

---

### ✨ 核心亮点

#### 🔄 四层叠加 PPT 转换

不是简单的"PDF截图贴进PPT"，而是四层独立解析、精准叠加，确保输出的每一个元素都可编辑：

| 层级 | 内容 | 效果 |
|------|------|------|
| Layer 1 | 位图（PNG/JPEG） | 保持原始图片的清晰度和格式 |
| Layer 2 | 矢量图（SVG） | Office 2019+ 可直接编辑路径和形状 |
| Layer 3 | 表格（原生 Table） | 行列可编辑，支持调整格式和内容 |
| Layer 4 | 文本框（TextBox） | 可编辑文字，保留字体/颜色/加粗/斜体 |

> 最终效果：打开 PPT 后，文字可选中编辑，表格可增删行列，图形可缩放旋转——而不是一张"死图"。

#### 📊 三引擎表格提取

面对复杂表格，单一引擎往往顾此失彼。mu-pdf-converter 采用三引擎级联策略，自动降级保障成功率：

| 优先级 | 引擎 | 擅长场景 |
|--------|------|----------|
| E1 | XY-Cut（pypdfium2） | 无边框表格、复杂合并单元格 |
| E2 | MarkItDown（AI辅助） | 非标准布局、松散对齐的数据 |
| E3 | pdfplumber | 标准有线框表格，兼容性兜底 |

> E1 失败自动尝试 E2，E2 失败降级 E3，三级兜底确保永不空手而归。

#### 🌍 外文 PDF 自动翻译

PDF 转 PPT 时自动检测外文内容，无需手动操作即可获得中文版：

- **智能语言检测**：CJK 字符占比 < 5% 自动判定为外文
- **专有名词保护**：技术术语、品牌名、代码片段不会被错误翻译
- **双引擎翻译**：translators 库优先（Google/百度/阿里级联）→ 本地 LLM（Ollama / OpenAI-compatible API）降级
- **批量合并请求**：减少 API 调用次数，大文档翻译效率提升数倍

#### 📝 双路径表单填写

支持两种 PDF 表单场景，覆盖所有常见表单类型：

| 路径 | 适用场景 | 原理 |
|------|----------|------|
| 路径 A：字段填写 | 带可填字段的标准 PDF 表单 | 解析 AcroForm 字段，精准写入值 |
| 路径 B：坐标注释 | 无可填字段的"扁平"PDF 表单 | AI 分析页面图片识别位置 → 坐标注释叠加文字 |

> 路径 B 特别适合那些"看起来是表格但其实是纯图片"的 PDF，先转图分析位置，再精确注释。

#### 🧹 4 种水印清除策略

不只是简单模式匹配，提供多策略组合应对各类水印：

| 策略 | 说明 |
|------|------|
| Text 模式 | 识别并移除旋转/半透明的文字水印 |
| Image 模式 | 检测并移除图片型水印（Logo/印章） |
| XObject 模式 | 清理 PDF 内部 XObject 层的水印引用 |
| Aggressive 模式 | 综合三种策略 + 启发式规则，处理顽固水印 |

> 支持 `--detect-only` 预览模式，先看看检测到了什么，确认后再执行移除。

#### 🔌 MCP Server 原生支持

内置 MCP（Model Context Protocol）Server，可被 AI 助手直接调用，实现无缝集成：

- 协议：JSON-RPC 2.0 over stdin/stdout
- 零外部依赖：仅使用 Python 标准库实现 RPC 层
- 暴露 6 个工具：`pdf_to_pptx` / `pdf_to_docx` / `pdf_to_xlsx` / `pdf_to_images` / `pdf_fill_form` / `pdf_remove_watermark`

---

### 📌 与同类工具对比

| **维度** | **mu-pdf-converter** | **Adobe Acrobat** | **SmallPDF** | **iLovePDF** | **pdf2docx** |
|---|---|---|---|---|---|
| PDF→PPT 可编辑程度 | ✅ 四层叠加，文字/表格/图形均可编辑 | ✅ 较好 | ⚠️ 部分可编辑 | ⚠️ 部分可编辑 | ❌ 不支持 |
| 表格提取引擎 | 三引擎级联（XY-Cut→MarkItDown→pdfplumber） | 单引擎 | 单引擎 | 单引擎 | 单引擎 |
| 外文自动翻译 | ✅ 翻译+保留排版 | ❌ 无 | ❌ 无 | ❌ 无 | ❌ 无 |
| 批量处理 | ✅ 目录扫描，100文件批量 | ✅ 支持 | ⚠️ 有限 | ⚠️ 有限 | 需自行编码 |
| 表单填写 | ✅ 双路径（字段填写+坐标注释） | ✅ 支持 | ❌ 不支持 | ❌ 不支持 | ❌ 不支持 |
| 去水印 | ✅ 4种策略 | ✅ 支持 | ❌ 不支持 | ⚠️ 有限 | ❌ 不支持 |
| MCP Server | ✅ 原生支持 | ❌ 无 | ❌ 无 | ❌ 无 | ❌ 无 |
| 本地运行/隐私 | ✅ 100%本地 | ⚠️ 云端功能 | ❌ 上传云端 | ❌ 上传云端 | ✅ 本地 |
| 开源 | ✅ MIT | ❌ 商业软件 | ❌ 闭源 | ❌ 闭源 | ✅ 开源 |
| 费用 | 免费 | $239.88/年 | $9-18/月 | $7-12/月 | 免费 |

---

### 🚀 转换工作流

| 工作流 | 场景 | 触发词 |
|--------|------|--------|
| PDF → PPT | 高保真转换，四层叠加，外文自动翻译 | PDF转PPT、把PDF转成PPT、PDF to PPTX |
| PDF → Word | 文本段落+标题识别+表格保留 | PDF转Word、PDF转docx、这个PDF能编辑吗 |
| PDF → Excel | 三引擎表格提取，支持批量目录扫描 | PDF转Excel、提取PDF表格、批量提取表格 |
| PDF → 图片 | 每页高质量渲染，自定义DPI | PDF转图片、PDF转PNG、PDF截图 |
| 表单填写 | 双路径：可填字段 / 坐标注释 | 填写PDF表格、PDF表单填写 |
| 去水印 | 4种策略，支持预览模式 | PDF去水印、移除水印、去掉水印 |
| 外文翻译 | PDF转PPT时自动翻译外文内容 | 外文PDF翻译、英文PDF翻成中文 |

---

### ⚙️ 技术规格

| 项目 | 说明 |
|------|------|
| 运行环境 | Python 3.9+ |
| PDF 解析 | PyMuPDF (fitz) + pdfplumber |
| PPT 生成 | python-pptx + lxml（SVG 插入） |
| Word 生成 | python-docx |
| Excel 生成 | openpyxl |
| 表格检测 | XY-Cut v2 (pypdfium2) → MarkItDown → pdfplumber |
| 翻译引擎 | translators（多引擎级联）+ 本地 LLM（OpenAI-compatible API） |
| MCP 协议 | JSON-RPC 2.0 over stdin/stdout，零外部依赖 |
| 批量限制 | 单次 ≤ 100 个 PDF 文件、翻译 ≤ 5000 个文本块 |

---

### 🛠️ 快速开始

**第一步：安装依赖**

```bash
# 核心依赖（必装）
pip install pymupdf pdfplumber python-pptx python-docx openpyxl lxml pypdf

# 可选依赖（增强功能）
pip install pypdfium2 markitdown translators requests
```

**第二步：转换**

```bash
# PDF → PPT（外文自动翻译）
python scripts/pdf_to_pptx.py report.pdf --outfile report.pptx

# PDF → Excel（批量：提取目录内所有PDF表格）
python scripts/pdf_to_xlsx.py --batch ./invoices/ --outfile batch.xlsx

# PDF 去水印（先预览再执行）
python scripts/pdf_remove_watermark.py doc.pdf --detect-only
python scripts/pdf_remove_watermark.py doc.pdf --outfile clean.pdf
```

**第三步：MCP Server（可选）**

```bash
python scripts/mcp_server.py
```

> 💡 不安装可选依赖也能正常使用基础转换功能，仅 XY-Cut 表格引擎和翻译功能需要对应依赖。

---

### 🔒 安全与隐私

- **本地执行** — 所有转换脚本在本机执行，Skill 本身不会主动上传文件到任何第三方服务
- **AI Agent 提示** — 通过 AI Agent 使用时，对话内容会经过大模型处理，请注意敏感数据
- **无遥测** — 零追踪、零分析、零数据采集
- **外部调用可选** — 仅翻译功能可能调用外部 API（通过 `--no-translate` 完全关闭）
- **MIT License** — 完全开源，自由审计和修改

---

### ⭐ Star 趋势

如果这个工具帮你节省了时间，请给个 Star ⭐ — 帮助更多人发现它！

<a href="https://star-history.com/#mupoet/mu-pdf-converter&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=mupoet/mu-pdf-converter&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=mupoet/mu-pdf-converter&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=mupoet/mu-pdf-converter&type=Date" />
 </picture>
</a>

> 不是"上传到云端等结果"，是"本地四层解析让PDF里每个元素都活过来"。

---

### 👤 作者简介

🎓 清华大学出版社签约作家 / 2026当当影响力作家 / 某互联网大厂 AI 大模型业务 HR 砖家 / 一级人力资源管理师 / 二级心理咨询师 / 野生设计师

📚 著有[《图解团队管理》](https://item.m.jd.com/product/14547345.html)，服务客户有字节跳动、腾讯、百度、中国移动、SMG、BOE…

💡 [微信公众号](https://mp.weixin.qq.com/s/v1JSZvlN5fvbOOHvkvXEtA) / [小红书](https://xhslink.com/m/ESxtgUNMdl)：muippt

---

### 📄 许可证与致谢

[MIT](LICENSE) © 2024-2026 mupoet (木先生iPPT)

**致谢**：感谢 [PyMuPDF](https://github.com/pymupdf/PyMuPDF)、[python-pptx](https://github.com/scanny/python-pptx)、[pdfplumber](https://github.com/jsvine/pdfplumber)、[MarkItDown](https://github.com/microsoft/markitdown) 等优秀开源库提供底层能力支持。

> 声明：本项目大部分内容由 AI 辅助完成。如您认为您的作品被使用但未获得适当署名，请提交 issue。
