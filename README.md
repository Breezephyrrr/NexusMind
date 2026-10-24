# NexusMind — 本地大模型 Agent 应用

**版本 0.20 正式版（versionCode 30）** · 作者 **zephyr_风** · 个人网站 [breezephyr.gt.tc](https://breezephyr.gt.tc)

NexusMind 是一款完全离线运行大模型的 Android 应用：支持 GGUF 对话模型（含多模态）、Stable
Diffusion 绘图模型、技能（Skill）导入、Agent 工具调用，内置浏览器与 Linux 沙箱。
界面采用 Material 3 设计，支持深色/浅色模式切换与全面屏挖孔 edge-to-edge 适配。
应用支持**简体中文 / 繁體中文 / English** 三种界面语言。

> 🌍 English documentation: see [English Version](#english-version) below.

## 主要功能

| 模块 | 说明 |
|------|------|
| 💬 对话助手 | 加载本地 GGUF 模型流式对话，支持图片输入（多模态 mmproj），支持生成图片输出；**多会话管理**：输入栏上方会话按钮打开**右侧会话侧边栏**（点击切换 / 长按删除 / 一键新建），各会话独立持久化（首次启动自动迁移旧版历史）；**深度思考**：思考按钮可开关并调节思考深度（简短 low / 适中 medium / 深入 high / 极致 max），思考过程以独立折叠框流式展示（结束自动折叠，点击标题或内容轻点即可展开）；**消息内链接可点击**，直接用内置浏览器打开；顶部模型状态胶囊实时显示加载进度，生成中发送键一键变停止键；**云端模型（可选）**：支持 OpenAI 兼容 API（DeepSeek / OpenAI / 通义 / OpenRouter 等），同样具备工具调用与深度思考能力，软件核心仍为本地离线推理 |
| 🎨 绘图工坊 | stable-diffusion.cpp 引擎，支持 SD 1.x / SDXL 等，模型支持 **bin.gz（自动解压）/ gguf / safetensors** |
| 📦 模型管理 | 三类模型分目录存储、按文件名自动识别类型；导入 / 删除 / 切换当前模型时**自动卸载旧引擎释放内存**；导入对话模型前会展示 **uncensored 风险免责声明** |
| 🧩 技能中心 | 导入 JSON 技能文件，注入系统提示词并指定推荐工具，可随时启停；**默认内置「创建技能」技能**，agent 可通过 `save_skill` 工具把工作流程自主沉淀为技能 |
| 🤖 Agent 工具 | 模型通过 `<tool_call>` 协议调用 **27 种内置工具**，形成自主工作循环；**工具执行前可要求人工确认**（设置内开关）；**每个工具都有独立开关**（设置 → 工具管理），**只有启用的工具才注入提示词**，未开启的工具模型无法调用，兼顾性能与 token 占用 |
| 📚 本地知识库 | 导入 txt / md 等文本资料构建专属知识库（自动分块索引），Agent 通过 `search_knowledge` 工具**离线检索**——相当于联网搜索的本地版，无需联网即可回答资料相关问题，隐私数据不出设备 |
| 📁 工作区 | 侧边导航「工作区」页面：agent 的持久文件目录，可浏览 / 新建 / 重命名 / 删除文件，并把文件**发送到对话**（图片走多模态、文本拼入消息）；agent 用 `workspace_write / read / list / delete` 四种工具读写文件、保存报告与代码产物；默认应用私有目录，授权"所有文件访问"后可自定义路径 |
| 🔌 MCP 服务 | 在专业沙箱内运行 MCP（Model Context Protocol）服务器，为 Agent **动态扩展外部信息工具**（联网抓取、文件系统、数据库等，取决于所装服务包）；连接成功后工具自动注入提示词、发消息自动重连 |
| 🚀 硬件加速 | **GPU 加速（Vulkan）**：一键开启，模型全部层卸载到 GPU（引擎内置 Vulkan 后端，自动检测）；**NPU 加速（实验性）**：自动检测芯片（骁龙 8 系 / 天玑旗舰），**检测不到 NPU 时开关自动禁用** |
| 🐧 双沙箱 | **简洁版**：Alpine minirootfs（约 4MB，apk 装软件）；**专业版**：完整 Ubuntu 22.04 **预装系统**（APK 内置 rootfs，curl/git/python3+pip+requests/jq/ssh 等**全部工具与 CA 证书一并预装**，**切换即本地解压部署、全程零联网零下载、几秒即就绪**），Agent 可用 **apt/pip/npm 自主下载安装软件**、执行**注册登录等网络自动化操作**（对模型智商要求高，启用时会弹窗提醒） |
| 🌐 内置浏览器 | 基于 WebView 的完整浏览器，可搜索、可前进后退；对话内链接与搜索结果点击后在此打开 |
| ⚙️ 设置 | 卡片式分组布局；上下文长度、线程数、温度、top_p、工具轮数等参数均**直接在输入框中输入**（非法值自动还原）；**Agent 人设可自定义助手名字与回答风格**（所有模型通用）；**工具管理**（21 个工具独立开关 + 全选/全不选）；**搜索引擎**（默认 Bing，可切换 DuckDuckGo 或自定义 URL 模板，失败自动降级重试其他通道）；**硬件加速**（GPU Vulkan / NPU 开关）；**沙箱环境**（简洁 / 专业）；**云端模型**（API 地址 + Key + 模型名，可一键测试连接）；**界面语言**（简中 / 繁中 / English / 跟随系统）；Agent 开关、工具执行确认、模型卸载、主题 |
| ℹ️ 关于 | 版本、作者、协议全文（首次启动也会弹出协议确认） |

## Agent 工具列表（27 种内置 + MCP 动态扩展，均可独立开关）

**文件与沙箱**：`run_command`（Linux 沙箱执行命令，简洁/专业双模式，专业版 Ubuntu 环境支持 apt/pip/npm）、`list_files`、`read_file`、`write_file`、`save_note`、`read_note`

**工作区**：`workspace_write`（把报告 / 代码 / 笔记等产物写入工作区，用户可在工作区页面查看并发送到对话）、`workspace_read`、`workspace_list`、`workspace_delete`（路径相对工作区根，自动防目录穿越）

**信息获取**：`web_search`（联网搜索，Bing / DuckDuckGo / 自定义 URL 三通道，失败自动降级；结果在对话中渲染为可点击网站列表）、`read_webpage`（抓取网页正文）、`open_url`（用内置浏览器打开链接，Agent 搜索的备用通道）、`search_knowledge`（**本地知识库离线检索**：按相关度返回资料段落及来源标注，涉及已导入文档的问题优先使用）

**技能**：`save_skill`（把沉淀的工作流程保存为可复用技能，用户在技能页启停；配合默认内置的「创建技能」技能使用效果最佳）

**MCP 动态工具**：连接 MCP 服务后，以 `mcp_{服务名}_{工具名}` 命名自动注入（如 mcp-server-fetch 提供 `mcp_fetch_fetch` 抓取工具），随服务开关增减

**效率工具**：`calculator`（数学表达式计算）、`get_time`（当前日期时间/星期/时间戳）、`calendar`（某年某月日历）、`unit_convert`（长度/重量/温度/面积/体积/速度/数据单位换算）、`hash_text`（MD5/SHA1/SHA256/Base64 编解码）

**设备能力**：`weather`（全球城市天气与多日预报，基于 open-meteo，带网络超时）、`device_info`（电量/内存/屏幕/系统版本）、`flashlight`（手电筒开关）、`clipboard`（读取剪贴板）、`open_app`（打开手机上的应用）、`password_gen`（随机安全密码）、`generate_image`（本地 Stable Diffusion 绘图）

对话中模型输出形如：

```
<tool_call>{"name": "run_command", "arguments": {"command": "ls -la /root"}}</tool_call>
```

应用会执行命令并将结果以 `<tool_response>` 回填，模型继续推理，直至给出最终回答。

## 技能（Skill）文件格式

```json
{
  "name": "linux-helper",
  "description": "Linux 命令专家",
  "instructions": "你是一名 Linux 专家，回答尽量给出可直接执行的命令。",
  "tools": ["run_command", "read_file"],
  "enabled": false
}
```

将文件保存为 `.json` 后在「技能中心」导入，开启开关即生效。

## 模型格式说明

- **对话模型**：GGUF 格式（Qwen / Llama / GLM / Phi 等）；自带视觉编码器的单文件多模态 GGUF（Gemma 3 / Qwen-VL 合并版等）加载后可直接图片输入，纯文本模型不受影响
- **绘图模型**：Stable Diffusion 系列；`bin.gz` 导入时会自动解压，也支持 gguf / safetensors
- 建议手机内存 ≥ 8GB，加载 4bit 量化模型

## 0.20 更新

- **推理参数调节增强（全新）**：模型设置新增「推理参数（采样）」小节——**温度 / top_p / min_p 改用滑块拖动调节**（实时显示数值，松手保存），新增**重复惩罚 repeat_penalty**与**min_p 最小概率**两个参数（本地 llama.cpp 采样链对齐官方顺序：penalties → top_k → top_p → min_p → temp），一键**恢复默认推理参数**；参数修改后自动卸载模型，下次对话重载生效
- **修复思考框内无法滑动**：根因是外层 RecyclerView 在判定滑动时拦截了触摸事件，内层滚动容器永远收不到滚动手势；现对思考内容区与工具结果区的滚动容器在按下时即阻止外层拦截，同时滚动高度改为按实测内容钳制（短内容自适应、超长内容限高内滚动）——思考框与工具结果框内均可自由滑动，滑出边界后聊天列表继续滚动
- **顶部状态胶囊美化收窄**：限宽 260→190dp、字号微降、间距收紧、圆角胶囊增加细描边，长模型名中间省略
- **修复关于页 NexusMind 标题偏移**：头部图标 / 标题 / 版本胶囊 / 简介均显式声明居中，不再依赖父容器 gravity（部分机型下 TextView 默认左对齐导致标题偏移）
- **修复深度思考折叠框点击不灵敏**：折叠点击监听改为 ViewHolder 级一次注册（此前每次绑定重复设置，响应不稳定）；思考内容区轻点（非滑动）同样可折叠 / 展开；流式输出期间思考框实时同步出现、更新与折叠状态
- **消息内链接可点击（全新）**：AI 回复中的链接——Markdown `[文本](网址)` 与裸网址——点击后直接用**内置浏览器**打开；链接与主题色联动，段落 / 列表 / 引用 / 表格内均可用
- **搜索结果可点击列表（全新）**：`web_search` 的结果卡片渲染为**可滑动、可点击的网站列表**（标题 + 网址），点击任意一条直接用内置浏览器打开；搜索结果随会话持久化
- **Agent 自主创建技能（全新）**：新增 `save_skill` 工具，agent 可把沉淀的工作流程直接保存为技能；默认内置「创建技能」技能（启用后指导 agent 何时、如何创建技能）；技能创建后仍由用户在技能页启停
- **工作区（全新）**：侧边导航新增「工作区」——agent 的持久文件目录：浏览 / 新建文件夹 / 新建文件 / 重命名 / 删除，并可**把文件发送到对话**（图片走多模态输入、文本文件内容拼入消息供模型阅读）；新增 `workspace_write / workspace_read / workspace_list / workspace_delete` 四种工具，agent 可在工作区读写删文件、保存报告与代码等产物；默认位于应用私有目录（`filesDir/workspace`），授予"所有文件访问"权限后可自定义到任意路径；agent 系统提示词自动注入工作区路径
- **会话侧边栏（全新）**：会话列表从弹窗改为**右侧抽屉侧边栏**——点击切换、长按删除、右上角新建，生成中自动禁止操作
- **输入栏重排**：会话 / 思考 / 图片三个按钮移到输入框**上方一排**，输入多行长文本不再拥挤
- **双沙箱完全离线化**：Ubuntu 专业版升级为**预装完整系统**——rootfs 内预装 ca-certificates / curl / wget / git / python3 + pip + requests / jq / unzip / zip / nano / vim / procps / net-tools / ssh 全套工具，连同 **https 清华 apt 源、CA 根证书、DNS 配置**一并打包进 APK（assets 内置 rootfs 约 64MB）；**切换专业版时仅本地解压部署，全程零联网零下载，几秒即就绪**（此前：下载 27MB 基础系统 + 联网 apt 预装 2~5 分钟）；assets 缺失时仍自动回退在线镜像下载（4 源断点续传）
- **关于页重制**：应用图标居中 + 版本胶囊 + 卡片式分区（作者 / 技术栈 / 开源致谢 / 协议）
- **工具修复与排查**：`weather` 加网络超时（此前无超时会永久卡死工具线程）；`save_note` 补参数校验；全部 27 种内置工具逐一排查参数缺失与异常路径

## 0.18-alpha 更新

- **模型设置（全新）**：设置页新增「模型设置」分区——**自定义系统提示词**（多行编辑，追加在内置规则之后、优先级最高，下一条消息立即生效）、**top_k 候选数**新增，上下文长度 / 线程数 / temperature / top_p 集中管理
- **修复采样参数不生效**：temperature / top_p 此前修改后对本地模型不生效（参数在模型加载时固化），现在修改会自动卸载已加载模型，下次对话重载生效
- **多模态投影回归**：重新支持独立 mmproj 投影文件——模型管理恢复「多模态投影」类型与分组，导入文件名自动识别；视觉能力加载优先用主模型内置视觉编码器，未内置时自动回退投影文件，两者都没有则纯文本对话；设置 → 当前模型 可查看 / 清除投影配置
- **性能优化：Markdown 流式增量渲染**：AI 回复流式输出时只重建正在增长的末尾块，前面的块视图直接复用——长回复渲染开销大幅下降，流式更顺滑
- **修复工具结果显示不全**：结果高度限制从文本控件移到滚动容器（此前超出 240dp 的文本被直接裁掉且无法滚动查看），上限提升至 280dp；超长参数 JSON 折叠显示
- **工具调用优化**：`<tool_call>` JSON 解析容错增强（全角标点 / 尾随逗号自动修复）；工具名缺失时给出明确提示；工具结果回传给模型的长度上限 1500 → 4000 字符，复杂任务结果不再被过度截断
- **图片缩略图**：对话中上传的图片以 72dp 圆角缩略图直接显示在气泡上方（最多 4 张），取代原计数角标，后台降采样解码不卡顿
- **其他**：修复重复注册的国际化词条

## 0.17.1-alpha 更新

- **长按消息操作（全新）**：长按任意用户消息或 AI 回复弹出操作菜单——**重新生成**（从所选消息起重跑该轮对话，带图消息的图片一并重发）、**复制**（复制消息 Markdown 原文到剪贴板）、**删除**（删除用户消息连带整轮回复，删除 AI 回复连带同轮工具调用与生成图，附件文件同步清理）
- 重新生成完整复用发送流程：MCP 自动连接、深度思考、云端 / 本地模式、流式渲染与正常发送行为完全一致；生成期间长按操作自动禁用并提示
- **修复长按菜单无法触发**：禁用消息文本的系统长按选择（此前正文与思考内容的 textIsSelectable 会让系统"选择 / 复制"菜单拦截长按手势），长按消息任意位置——包括思考框、代码块、表格内——均可弹出操作菜单；查看原文改由菜单内「复制」一键复制全文

## 0.16-alpha 更新

- **界面大幅美化**：全新**手绘矢量图标体系**（对话、绘图、模型、技能、Agent、浏览器、沙箱、设置、关于等 17 枚，含侧边导航 / 设置分组 / 思考与工具卡片），替代原 emoji 前缀；图标随 Material 主题主色动态着色，侧栏选中项图标与文字同步高亮
- **「对话助手」更名 NexusMind**：侧边导航首项直接以应用品牌呈现
- **Markdown 渲染（全新）**：AI 回复不再按纯字符显示——自动渲染 **粗体**、*斜体*、~~删除线~~、`行内代码`、代码块（等宽字体 + 横向滚动 + 圆角底色）、无序 / 有序列表、引用块、分隔线、链接，以及**表格**（网格线、对齐支持、横向滚动）；零第三方依赖的自研轻量渲染器，渲染异常自动退回纯文本，流式输出过程中实时渲染
- **修复生成内容超出屏幕时的抖动**：流式刷新改为 payload 局部更新（只刷新文本区，不再整项重绑，工具确认区不再闪烁）；自动滚动由 scrollToPosition 改为**贴底算法**（精确滚动剩余距离）——此前 AI 回复高度超过一屏时，scrollToPosition 会把条目顶部对齐到视口顶部，与新内容增长互相拉扯造成跳帧，现长回复也稳定停在最新内容
- **移除 mmproj 多模态投影类型（架构简化）**：不再需要单独的投影文件——直接支持**自带视觉编码器的单文件多模态 GGUF**（Gemma 3 vision / Qwen-VL 等合并版，加载后自动启用图片输入），纯文本模型完全不受影响；模型管理简化为「对话 / 绘图」两类，导入类型选择与自动识别同步更新
- **其他**：工具卡片标题、用户带图消息、思考折叠框标题改用图标标记；清理文本内全部 emoji（错误提示、状态文案）

## 0.15-alpha 更新

- **界面三语言支持（全新）**：简体中文 / 繁體中文 / English，入口在 设置 → 外观 → 「界面语言」（默认跟随系统：繁体脚本或台湾/香港/澳门地区自动判定繁中）。全应用 **300+ 词条**覆盖所有界面——设置页、对话、绘图工坊、模型管理、技能、沙箱、浏览器、关于，以及沙箱安装 / MCP 连接等全部进度与错误提示，切换**即时生效**；未翻译词条自动回退简中，不会出现空白
- **使用协议双语化**：协议全文润色并改为 **简体中文 + English 双语版本**（协议版本 1.0），首次启动弹窗与「关于」页同步更新
- **多语言工程细节**：界面文案与状态判断同语言联动（工具执行状态着色、沙箱进度阶段判断在三语下均正常）；工具描述与参数文档保持英文不翻译（避免影响模型提示词行为）

## 0.14-alpha 更新

- **本地知识库（全新）**：设置 → 本地知识库，导入 txt / md 等文本资料（自动按段落滑窗分块建立索引，同名导入即覆盖）；Agent 新增 `search_knowledge` 工具，**纯本地离线检索**（中文 bigram + 英文分词混合匹配、按相关度排序返回段落与来源标注）——相当于联网搜索的离线版，笔记 / 资料 / 手册类问题无需联网即可回答，隐私数据不出设备；支持文档删除与分块统计
- **MCP 服务支持（全新）**：设置 → MCP 服务，可添加多个 MCP（Model Context Protocol）服务器，在**专业沙箱内**以 stdio 方式常驻运行；连接时自动完成 initialize 握手与 tools/list 工具发现，发现的工具以 `mcp_{服务名}_{工具名}` 命名注入 Agent 提示词；服务崩溃后调用自动重连一次，发消息时已启用服务自动重连（等待至多 4 秒保证工具注入本次对话）；内置示例：`pip install mcp-server-fetch` + 命令 `python3 -m mcp_server_fetch` 即可为 Agent 扩展网页抓取能力
- **Agent 工具增至 22 种**（新增 search_knowledge），MCP 工具动态扩展不占独立开关位

## 0.13-alpha 更新

- **深度思考框可滑动**：思考内容限高 240dp（与工具结果一致），max 深度的长思考在卡片内上下滑动查看，不再撑爆整个列表项
- **绘图工具超时保护**：模型加载 / 图像生成分别加 3 / 10 分钟超时，native 引擎异常卡死时返回明确错误提示，不再永久锁死对话流程（此前只能重启应用）

## 0.12-alpha 更新

- **修复专业沙箱"配置 apt 源卡住"**：Ubuntu base 未预装 ca-certificates，https 的 apt 源在证书校验阶段必然失败/卡死 —— 首次改用 **http 源**完成预装（装上证书后自动升级回 https 源）；预装包移除 man-db（其安装脚本触发 mandb 全库索引，proot 下极慢）；DNS 增加腾讯/阿里公共节点
- **预装过程实时可见**：apt 预装拆分为「更新软件源索引 → 安装工具」两段独立超时，失败可定位到具体环节；安装期间**监视 apt 日志尾行实时回显**（正在下载/解包哪个包一目了然），不再长时间静止转圈；失败时展示 apt 真实错误尾行

## 0.11-alpha 更新

- **专业沙箱下载修复**：实测中科大镜像已 403 失效（移除），换为清华 TUNA → 阿里云 → 华为云 → Ubuntu 官方四源降级；新增**断点续传**（部分下载缓存在本地，重试 / 换镜像从已下载字节继续，弱网不再"一失败就全部重来"）；下载完整度按 Content-Range 校验
- **实时安装进度**：启用专业沙箱后弹出进度对话框（进度条 + 阶段文本 + 已下载/总大小 + 当前镜像源），取代此前 Toast 连击；安装中重复触发自动防重入；设置新增「专业沙箱安装 / 修复」入口（显示实时状态，随时可重试）
- **修复工具结果无法滑动**：0.10 使用的 TextView MovementMethod 会被列表滚动拦截，现改为 NestedScrollView 承载结果区（嵌套滚动协议：结果区内自由滑动，滑到边界后聊天列表继续滚动）

## 0.10-alpha 更新

- **思考格式还原**：确认模型深度思考输出为标准 `<think>…</think>` 格式，撤回 0.08 的闭标签开场兼容逻辑（解析行为恢复：`<think>`/`<thinking>` 开标签 + DeepSeek 无开标签兜底）；「关闭思考强制隐藏」与「思考区外才检测工具调用」两项修复保留
- **工具结果卡片可滑动**：工具执行结果（搜索结果、命令输出等）上限改为 240dp 高度，超出部分在卡片内**上下滑动查看**，不再截断丢失内容
- **新增搜索结果数量设置**：设置 → 搜索引擎 → 「搜索结果数量」，可选 5 / 8 / 10 / 15 条，所有搜索通道（Bing 主站 / RSS / DuckDuckGo）统一生效
- **动画与流畅度**：恢复列表插入/移除的自然动画（150ms，流式刷新仍无抖动）；顶部模型状态胶囊淡入；深度思考折叠框展开/收起过渡动画

- **修复"关闭思考不生效"**：部分模型不受提示词约束、关闭后仍输出 think 标签。现于解析层强制兜底（本地与云端一致）：思考开关关闭时，模型输出的思考内容一律隐藏，只展示正文 —— 无论模型是否听话，界面行为与开关严格一致
- **修复思考弹窗逻辑**：关闭"启用深度思考"后，思考深度选择区整块置灰禁用，不可再调节（此前可操作但无意义）
- **修复搜索工具失效**：Bing RSS 接口被官方限制为仅返回 1 条结果（原主通道），DuckDuckGo 在国内网络无法直连。现重构搜索链：**Bing 主站 HTML 解析为主通道**（cn.bing / www.bing 互备，实测返回完整多条结果），RSS 降为兜底，再依次降级 DuckDuckGo / 自定义引擎
- **工具调用健壮性**：模型在思考内容中提及 `<tool_call>` 字样不再被误判为真实调用（仅在思考区块外检测）；21 个工具逐一排查，补齐参数缺失校验（漏传参数返回明确错误提示而非 "null"）
- **其他**：补齐手电筒 FLASHLIGHT 权限（部分机型此前会静默失败）；绘图工具宽高上限与说明文档统一为 512

## 0.08-alpha 更新

- **修复深度思考框偶尔不显示**：部分模型输出的思考标签开头与结尾都是 `</think>`（即 `</think>思考内容</think>正文`，开标签被误写成闭标签），旧解析器因匹配不到 `<think>` 开标签而把整段思考当作正文显示。现已在本地与云端两条解析链兼容该格式：开头即是 `</think>` 时自动视作开标签并与后续闭标签正确配对，思考内容正常进入折叠框

## 0.07-alpha 更新

- **修复严重闪退**：上一版本（0.06）部分设备上「加载模型或对话即闪退」。根因为 Vulkan 后端编译时缺少异常支持（NDK 默认 `-fno-exceptions`），引擎初始化抛出的异常直接触发 `std::terminate`。现已修复，并增加 GPU 安全门：Vulkan 后端默认待机，仅在设置中开启 GPU 加速后才初始化设备枚举，彻底杜绝初始化期崩溃；GPU 开关可随时热切换
- **新增多会话**：输入栏左侧新增会话按钮，支持新建 / 切换 / 长按删除会话；每个会话独立保存于应用私有目录（`sessions/{id}.json`），标题自动取首条提问；旧版本的单文件聊天记录会在首次启动时自动迁移为第一个会话；生成回复期间暂停会话操作，避免状态错乱

## ⚠️ 安全声明

> 最好不要使用 uncensored（无审查）等去限制的模型来调用工具和执行命令，否则可能发生不受约束的
> 破坏性行为（删除文件、危险命令等），由此产生的一切后果开发者概不负责。

## 构建

```
./gradlew assembleRelease   # 需要 JDK 17+ 与 Android SDK（minSdk 28，Android 9+）
```

- 推理引擎：llama.cpp 最新 master（2026-09 重新编译，arm64-v8a），**内置 Vulkan GPU 后端**（GGML_VULKAN，设置中可开关）+ mtmd 多模态；支持 2025H2 后新模型架构；加载失败时展示 llama.cpp 真实错误日志
- 源码包不含 `prebuilt/` 预编译库（libllama/libggml/libggml-vulkan/libmtmd/libstable-diffusion 等），需自行编译后放入 `app/src/main/cpp/prebuilt/`
- 绘图引擎：stable-diffusion.cpp
- Linux 沙箱：简洁版 Alpine minirootfs 3.20（内置 assets）+ 专业版 Ubuntu 22.04 **预装完整系统**（工具/证书/源全内置 assets，**切换即部署零联网**，assets 缺失时回退在线下载）+ Termux proot

---

# English Version

**Version 0.20 Stable (versionCode 30)** · Author **zephyr_风** · Website [breezephyr.gt.tc](https://breezephyr.gt.tc)

NexusMind is an Android app that runs large language models **fully offline**: GGUF chat models (multimodal supported), Stable Diffusion image models, Skill imports, Agent tool calling, a built-in browser, and Linux sandboxes.
The UI uses Material 3 with dark/light themes and edge-to-edge cutout support.
Interface languages: **Simplified Chinese / Traditional Chinese / English**.

## Key Features

| Module | Description |
|--------|-------------|
| 💬 Chat Assistant | Streamed chat with local GGUF models; image input (multimodal mmproj) and image output; **multi-session management** via a **right-hand session drawer** (tap to switch / long-press to delete / one-tap create, each session persisted independently, legacy history auto-migrated); **deep thinking** with adjustable depth (low / medium / high / max) shown in a collapsible streaming box (tap the header or the content to toggle); **clickable in-message links** opening in the built-in browser; model status pill shows load progress in real time, send button becomes a stop button while generating; **cloud models (optional)**: any OpenAI-compatible API (DeepSeek / OpenAI / Qwen / OpenRouter…) with tool calling and deep thinking — the core remains local inference |
| 🎨 Image Studio | stable-diffusion.cpp engine, SD 1.x / SDXL; models accepted as **bin.gz (auto-extracted) / gguf / safetensors** |
| 📦 Model Manager | Three model categories in separate folders, auto-detected by filename; switching / deleting / importing **auto-unloads the previous engine to free memory**; an **uncensored-model disclaimer** is shown before importing chat models |
| 🧩 Skill Center | Import JSON skill files that inject system prompts and recommended tools; toggle anytime; ships with a built-in "Create skill" skill — the agent can persist workflows as skills via the `save_skill` tool |
| 🤖 Agent Tools | The model calls **27 built-in tools** via the `<tool_call>` protocol in an autonomous loop; **human approval before execution** (toggle in settings); **per-tool switches** (Settings → Tools) — only enabled tools are injected into the prompt, saving tokens and compute |
| 📚 Local Knowledge Base | Import txt / md documents to build a private knowledge base (auto chunked & indexed); the Agent searches it offline via `search_knowledge` — a local alternative to web search; answers stay on-device |
| 📁 Workspace | A new "Workspace" page in the navigation: the agent's persistent file directory — browse / create / rename / delete files and **send them into the chat** (images via multimodal input, text inlined into the message); the agent uses `workspace_write / read / list / delete` to save reports, code and artifacts; defaults to app-private storage, relocatable after granting "All files access" |
| 🔌 MCP Services | Run MCP (Model Context Protocol) servers inside the Pro sandbox to **dynamically extend the Agent** with external tools (web fetch, filesystem, databases — depending on installed packages); discovered tools are auto-injected and services auto-reconnect on each message |
| 🚀 Hardware Acceleration | **GPU (Vulkan)**: one tap offloads all layers to the GPU (backend built in, auto-detected); **NPU (experimental)**: auto-detects SoC (Snapdragon 8 / Dimensity flagships); the toggle is disabled when no NPU is found |
| 🐧 Dual Sandbox | **Simple**: Alpine minirootfs (~4MB, install apps via apk); **Pro**: full Ubuntu 22.04 **preinstalled system** (rootfs bundled in the APK with curl/git/python3+pip+requests/jq/ssh and **all tools & CA certs preinstalled — deploys locally on switch, zero download, ready in seconds**), the Agent can **install software via apt/pip/npm** and perform **web automation such as sign-ups/logins** (demanding; a reminder dialog appears when enabled) |
| 🌐 Built-in Browser | Full WebView browser with search, back/forward; opens in-message links and tappable search results |
| ⚙️ Settings | Card-grouped layout; context size, threads, temperature, top_p, tool rounds typed directly into fields (invalid values auto-revert); **custom Agent persona** (name & style, works with any model); **tool manager** (per-tool switches + select all/none); **search engine** (Bing default, DuckDuckGo or custom URL template, auto-fallback on failure); **hardware acceleration** (Vulkan / NPU); **sandbox** (Simple / Pro); **cloud model** (URL + key + model name, one-tap test); **interface language** (Simplified / Traditional Chinese, English, or system); Agent toggle, tool approval, model unload, theme |
| ℹ️ About | Version, author, and the full user agreement (also shown on first launch) |

## Agent Tools (27 built-in + dynamic MCP, each independently switchable)

**Files & sandbox**: `run_command` (execute commands in the Linux sandbox — Simple/Pro modes; the Pro Ubuntu environment supports apt/pip/npm), `list_files`, `read_file`, `write_file`, `save_note`, `read_note`

**Workspace**: `workspace_write` (save reports / code / notes as artifacts the user can view in the Workspace page and send into the chat), `workspace_read`, `workspace_list`, `workspace_delete` (paths relative to the workspace root, traversal-protected)

**Information**: `web_search` (Bing / DuckDuckGo / custom URL with automatic fallback; results render as a tappable website list in chat), `read_webpage` (extract article text), `open_url` (open in the built-in browser as a fallback search path), `search_knowledge` (**offline retrieval over the local knowledge base**: returns ranked passages with source notes; preferred for questions about imported documents)

**Skills**: `save_skill` (persist a workflow as a reusable skill; users toggle it in the Skill Center; works best with the built-in "Create skill" skill)

**Dynamic MCP tools**: after connecting an MCP service, tools are injected as `mcp_{service}_{tool}` (e.g. mcp-server-fetch provides `mcp_fetch_fetch`); they appear/disappear with the service toggle

**Productivity**: `calculator`, `get_time` (date/time/weekday/timestamp), `calendar`, `unit_convert` (length/weight/temperature/area/volume/speed/data), `hash_text` (MD5/SHA1/SHA256/Base64)

**Device**: `weather` (worldwide forecasts via open-meteo, with network timeouts), `device_info` (battery/memory/screen/OS), `flashlight`, `clipboard`, `open_app`, `password_gen`, `generate_image` (local Stable Diffusion)

In chat, the model emits:

```
<tool_call>{"name": "run_command", "arguments": {"command": "ls -la /root"}}</tool_call>
```

The app executes the command and feeds the result back as `<tool_response>`; the model keeps reasoning until it produces the final answer.

## Skill File Format

```json
{
  "name": "linux-helper",
  "description": "Linux command expert",
  "instructions": "You are a Linux expert. Prefer commands that can be executed directly.",
  "tools": ["run_command", "read_file"],
  "enabled": false
}
```

Save as `.json`, import in the Skill Center, and toggle it on.

## Model Formats

- **Chat models**: GGUF (Qwen / Llama / GLM / Phi…); single-file multimodal GGUFs with a built-in vision encoder (merged Gemma 3 / Qwen-VL etc.) support image input right after loading, plain-text models unaffected
- **Image models**: Stable Diffusion family; `bin.gz` is auto-extracted on import; gguf / safetensors also supported
- Recommended: ≥ 8GB RAM for 4-bit quantized models

## 0.20 Changelog

- **Enhanced inference parameter controls (new)**: the "Inference parameters (sampling)" subsection joins Model settings — **temperature / top_p / min_p now use sliders** (live value readout, saved on release), plus two new parameters: **repetition penalty (repeat_penalty)** and **min_p minimum probability** (local llama.cpp sampler chain now matches the official order: penalties → top_k → top_p → min_p → temp); a one-tap **Reset inference parameters** restores defaults; changes auto-unload the model and apply on the next load
- **Fixed scrolling inside the thinking box**: the root cause was the outer RecyclerView stealing the touch event once it detected a scroll gesture, so the inner scroll container never received it; the thinking-content and tool-result scroll containers now block outer interception on touch-down, and scroll height is clamped by measured content (short content auto-fits, long content scrolls within a capped height) — both boxes scroll freely, and the chat list resumes scrolling past the edges
- **Status pill refined & narrowed**: max width 260→190dp, slightly smaller type, tighter spacing, thin outline on the rounded pill; long model names ellipsize in the middle
- **Fixed the NexusMind title offset on the About page**: the icon / title / version pill / tagline are all explicitly centered instead of relying on the parent's gravity (TextView's default left alignment skewed the title on some devices)
- **Fixed unresponsive deep-thinking toggle**: the collapse click listener is now registered once per ViewHolder (previously re-registered on every bind, making taps unreliable); a quick tap (not a scroll) on the thinking content also collapses / expands; the thinking box now appears, updates and keeps its collapsed state live during streaming
- **Clickable links in messages (new)**: links in AI replies — Markdown `[text](url)` and bare URLs — open directly in the **built-in browser**; themed link color works in paragraphs, lists, quotes and tables
- **Clickable search results (new)**: the `web_search` result card now renders a **scrollable, tappable list of websites** (title + URL); tapping any entry opens it in the built-in browser; results persist with the session
- **Agent-created skills (new)**: new `save_skill` tool lets the agent persist workflows as reusable skills; the built-in "Create skill" skill (enabled by default) teaches the agent when and how; users still toggle skills on/off in the Skill Center
- **Workspace (new)**: a new "Workspace" page in the side navigation — the agent's persistent file directory: browse / create folders / create files / rename / delete, and **send files into the chat** (images go through multimodal input, text files are inlined into the message); four new tools `workspace_write / workspace_read / workspace_list / workspace_delete` let the agent create, read, update and delete files, saving reports, code and other artifacts; defaults to app-private storage (`filesDir/workspace`), can be moved anywhere after granting "All files access"; the workspace path is injected into the agent's system prompt
- **Session drawer (new)**: the session list moved from a dialog to a **right-hand drawer sidebar** — tap to switch, long-press to delete, new-session button on top; operations pause while generating
- **Input bar relayout**: the session / thinking / image buttons moved to a **row above the input field**, so typing long text is no longer cramped
- **Both sandboxes fully offline**: Ubuntu Pro is now a **fully preinstalled system** — the bundled rootfs (≈64MB in assets) ships with ca-certificates / curl / wget / git / python3 + pip + requests / jq / unzip / zip / nano / vim / procps / net-tools / ssh, plus the **https TUNA apt source, CA root bundle and DNS config**; **switching deploys it locally with zero downloads — ready in seconds** (previously: ~27MB download + 2-5 min online apt preinstall); falls back to online mirrors (4 sources, resumable) if the asset is missing
- **About page redesigned**: centered app icon + version pill + card-style sections (author / tech stack / open-source credits / agreement)
- **Tool fixes & audit**: `weather` now has network timeouts (previously it could hang the tool thread forever); `save_note` argument validation added; all **27 built-in tools** audited for missing-argument and error paths

## 0.18-alpha Changelog

- **Model settings (new)**: new "Model settings" section in Settings — **custom system prompt** (multi-line, appended after built-in rules with the highest priority, effective on the next message) and new **top_k** control, alongside context size / threads / temperature / top_p
- **Fix: sampling params not applying**: temperature / top_p used to be ignored for local models (params are fixed at model load); changing them now auto-unloads the loaded model and applies on the next conversation
- **Multimodal projector is back**: standalone mmproj projector files supported again — the "Multimodal projector" type and group return to Model Manager with filename auto-detection; vision loading prefers the model's built-in vision encoder, falls back to the projector file, plain-text chat otherwise; view / clear the projector under Settings → Current model
- **Performance: incremental streaming Markdown**: while a reply streams in, only the growing tail block is rebuilt and earlier block views are reused — much lower render cost on long replies
- **Fix: tool results truncated**: the height cap moved from the text view to the scroll container (text beyond 240dp was cut off with no way to scroll), cap raised to 280dp; overlong argument JSON now collapses
- **Tool-call improvements**: `<tool_call>` JSON parsing is more forgiving (auto-fixes fullwidth punctuation / trailing commas); a clear hint shows when the tool name is missing; tool results fed back to the model raised from 1500 to 4000 chars
- **Image thumbnails**: attached images now show as 72dp rounded thumbnails above the message bubble (up to 4), replacing the count badge; decoded off-thread with downsampling
- **Other**: removed a duplicated i18n entry

## 0.17.1-alpha Changelog

- **Long-press message actions (new)**: long-press any user message or AI reply to open an action menu — **Regenerate** (re-runs the turn from the selected message, attached images resent), **Copy** (copies the raw Markdown text to the clipboard), **Delete** (deleting a user message removes the whole turn; deleting an AI reply also removes its tool calls and generated images, attachment files cleaned up)
- Regeneration reuses the normal send pipeline: MCP auto-connect, deep thinking, cloud / local modes and streaming render behave exactly like a regular send; actions are disabled with a hint while generating
- **Fix: long-press menu not showing**: system text selection is now disabled on message text (previously textIsSelectable on the body and thinking content let the system "select / copy" menu swallow the long-press); long-pressing anywhere on a message — including the thinking box, code blocks and tables — opens the action menu; use "Copy" in the menu to grab the full text

## 0.16-alpha Changelog

- **Major UI polish**: a brand-new **hand-drawn vector icon set** (17 icons covering navigation, settings sections, thinking box and tool cards) replaces emoji prefixes; icons tint dynamically with the Material theme, and the selected drawer item highlights icon + label together
- **"Chat Assistant" renamed to NexusMind**: the first navigation entry now shows the app brand directly
- **Markdown rendering (new)**: AI replies are no longer raw text — automatic rendering of **bold**, *italic*, ~~strikethrough~~, `inline code`, code blocks (monospace + horizontal scroll + rounded background), bullet / numbered lists, quotes, dividers, links, and **tables** (grid lines, alignment, horizontal scroll); a dependency-free lightweight renderer with plain-text fallback, re-rendered live during streaming
- **Fixed jitter when generated content outgrows the screen**: streaming updates now use payload-based partial rebinding (text-only; tool confirm rows no longer flash); auto-scroll switched from scrollToPosition to a **stick-to-bottom algorithm** (scroll the exact remaining distance) — previously an AI reply taller than the viewport got its top aligned to the viewport top, fighting the growing content and causing frame jumps; long replies now stay pinned to the newest content
- **Removed the mmproj projector type (architecture simplification)**: no separate projector files needed — **single-file multimodal GGUFs with a built-in vision encoder** (Gemma 3 vision / Qwen-VL merges) enable image input automatically on load; text-only models are entirely unaffected; model management is simplified to Chat / Image categories
- **Misc**: tool cards, user image flags and the thinking header now use icons; all in-text emoji removed from errors and status messages

## 0.15-alpha Changelog

- **UI localization (new)**: Simplified Chinese / Traditional Chinese / English via Settings → Appearance → Interface language (defaults to system; Traditional Chinese is auto-detected from the Hant script or TW/HK/MO region). **300+ entries** cover every screen — settings, chat, Image Studio, model manager, skills, sandbox, browser, About — plus all sandbox-install / MCP progress and error messages; switching takes effect **immediately**, and untranslated entries fall back to Simplified Chinese instead of showing blanks
- **Bilingual user agreement**: the agreement was polished and rewritten as a **Simplified Chinese + English** document (version 1.0); the first-launch dialog and About page are updated accordingly
- **Engineering notes**: status-coloring and progress-stage detection are language-aware (tool states and sandbox stages behave identically in all three languages); tool descriptions/argument docs stay in English on purpose — they are part of the model's prompt

## 0.14-alpha Changelog

- **Local knowledge base (new)**: Settings → Local Knowledge Base; import txt / md files (auto chunked with sliding windows, re-import replaces by name); new `search_knowledge` Agent tool with **fully offline retrieval** (Chinese bigram + English tokenization, ranked passages with source notes) — an offline alternative to web search; document removal and chunk stats supported
- **MCP services (new)**: Settings → MCP Services; add multiple MCP servers that run persistently via stdio **inside the Pro sandbox**; initialize handshake and tools/list discovery on connect; discovered tools are injected as `mcp_{service}_{tool}`; auto-reconnect after a crash (one retry per call) and on each message (up to 4s wait so tools make it into the current prompt); built-in example: `pip install mcp-server-fetch` + command `python3 -m mcp_server_fetch`
- **Agent tools now 22** (added search_knowledge); dynamic MCP tools don't occupy per-tool switch slots

## 0.13-alpha Changelog

- **Scrollable thinking box**: thinking content capped at 240dp (same as tool results); long max-depth thinking scrolls inside the card instead of blowing up the list item
- **Image tool timeouts**: 3 / 10 minute timeouts for model load / image generation; a hung native engine now returns a clear error instead of permanently blocking the chat (previously only an app restart helped)

## 0.12-alpha Changelog

- **Fixed Pro sandbox hang at "configuring apt sources"**: the Ubuntu base lacks ca-certificates, so https apt sources always fail at certificate verification — preinstall now uses **http sources first** (auto-upgrades to https once certificates are installed); man-db removed from preinstalls (its script triggers a mandb full index, extremely slow under proot); Tencent/Aliyun public DNS added
- **Live preinstall progress**: apt preinstall split into two independently-timed stages ("refresh index" → "install tools") so failures pinpoint the stage; the **tail of apt's log is streamed live** (you can see which package is being downloaded/unpacked) instead of a silent spinner; real apt error tail is shown on failure

## 0.11-alpha Changelog

- **Pro sandbox download fix**: the USTC mirror is dead (403, removed); now falls back through TUNA → Aliyun → Huawei → Ubuntu official; added **resumable downloads** (partial data is cached locally, retries/switches continue from downloaded bytes — flaky networks no longer restart from scratch); completeness verified via Content-Range
- **Real-time install progress**: a progress dialog (bar + stage text + downloaded/total + current mirror) replaces Toast spam; re-entry during install is blocked; a "Pro sandbox install / repair" entry was added to settings with live status and retry
- **Fixed unscrollable tool results**: the 0.10 TextView MovementMethod intercepted list scrolling; results now live in a NestedScrollView (nested scrolling protocol: scroll freely inside the result, then the chat list takes over at the edges)

## 0.10-alpha Changelog

- **Thinking format revert**: confirmed models emit standard `<think>…</think>`; reverted the 0.08 closing-tag-first compat logic (parsing back to `<think>`/`<thinking>` open tags + DeepSeek's missing-open-tag fallback); the "force-hide when thinking is off" and "detect tool calls only outside thinking" fixes are kept
- **Scrollable tool result cards**: results (search output, command output…) capped at 240dp with **in-card vertical scrolling** instead of truncation
- **Search result count setting**: Settings → Search engine → result count (5 / 8 / 10 / 15), applies to all channels (Bing web / RSS / DuckDuckGo)
- **Animations & smoothness**: natural list insert/remove animations restored (150ms, streaming stays jank-free); model status pill fades in; thinking box expand/collapse transitions

- **Fixed "disabling thinking doesn't work"**: some models ignore the prompt and still emit think tags. Now enforced at the parser layer (local & cloud): with the toggle off, thinking content is always hidden and only the answer is shown — UI behavior matches the toggle exactly
- **Fixed thinking dialog logic**: with "enable deep thinking" off, the depth selector is fully disabled (previously operable but meaningless)
- **Fixed broken search tool**: Bing RSS is now limited to 1 result by the official API (former main channel) and DuckDuckGo is unreachable from mainland networks. Rebuilt the chain: **Bing web HTML parsing as the main channel** (cn.bing / www.bing redundancy, verified to return full results), RSS as fallback, then DuckDuckGo / custom engine
- **Tool-calling robustness**: `<tool_call>` mentioned inside thinking content is no longer mistaken for a real call (detection only outside thinking blocks); all 21 tools audited with proper argument validation (missing args return clear errors instead of "null")
- **Misc**: added the FLASHLIGHT permission (some devices silently failed before); image tool max dimensions unified to 512

## 0.08-alpha Changelog

- **Fixed deep-thinking box occasionally not showing**: some models output `</think>` both at the start and end (`</think>thinking</think>answer` — the open tag miswritten as a close tag); the old parser found no `<think>` open tag and treated everything as the answer. Both local and cloud parsers now accept a leading `</think>` as an open tag and pair it with the closing tag correctly

## 0.07-alpha Changelog

- **Fixed critical crash**: on 0.06 some devices crashed on model load or chat. Root cause: the Vulkan backend was compiled without exception support (NDK default `-fno-exceptions`), so an exception during engine init hit `std::terminate`. Fixed, plus a GPU safety gate: the Vulkan backend idles by default and only enumerates devices when GPU acceleration is enabled in settings — initialization crashes are eliminated; the GPU toggle hot-switches anytime
- **Multi-session**: new session button on the left of the input bar (create / switch / long-press delete); sessions are stored independently in the app's private directory (`sessions/{id}.json`) with titles taken from the first prompt; legacy single-file history auto-migrates into the first session on first launch; session operations pause while generating to avoid state corruption

## ⚠️ Security Notice

> Avoid using uncensored models for tool calling and command execution. Unconstrained destructive behavior (file deletion, dangerous commands, etc.) may occur, and the developer takes no responsibility for any resulting damage.

## Build

```
./gradlew assembleRelease   # Requires JDK 17+ and the Android SDK (minSdk 28, Android 9+)
```

- Inference engine: llama.cpp latest master (rebuilt 2026-09, arm64-v8a) with the **Vulkan GPU backend built in** (GGML_VULKAN, toggle in settings) + mtmd multimodal; supports model architectures from 2025H2 onward; load failures show llama.cpp's real error log
- Source packages exclude the `prebuilt/` static libraries (libllama/libggml/libggml-vulkan/libmtmd/libstable-diffusion…); build them yourself and place them under `app/src/main/cpp/prebuilt/`
- Image engine: stable-diffusion.cpp
- Linux sandbox: Simple = Alpine minirootfs 3.20 (bundled in assets) + Pro = Ubuntu 22.04 **fully preinstalled** (tools/certs/sources all bundled — **deploys on switch with zero downloads**; online fallback if the asset is missing) + Termux proot
