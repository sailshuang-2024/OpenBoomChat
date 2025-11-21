# BoomChat
> 一款基于 Electron + React 的跨平台桌面聊天应用，专注于提供流畅的实时通信体验与多端数据同步能力，支持动态分享等核心社交功能。本项目开源范围不包含端到端加密具体实现、密钥管理及安全防护策略，仅提供客户端基础架构与功能模块（非加密相关）。

[English Version](#boomchat-english) | [许可证](#license) | [贡献指南](#contribution-guide)

---

## 一、项目介绍
### 1. 核心定位
BoomChat 旨在打造轻量、高效、易二次开发的桌面聊天解决方案，适用于个人日常沟通、团队内部协作场景，或作为 Electron + React 技术栈的实战学习范本。核心设计理念是「模块化、可扩展、跨平台兼容」，通过解耦业务逻辑与底层架构，降低二次开发门槛。

### 2. 核心特点
- **跨平台兼容**：深度适配 Windows 10+/macOS 12+/Linux（Ubuntu 20.04+/CentOS 8+），基于 Electron 的原生能力抽象层，统一 UI 与交互体验，避免平台特异性代码冗余。
- **技术栈成熟**：基于 Electron 38.x（稳定版，内置 Chromium 120+）+ React 18.x（Hooks 语法）+ Tailwind CSS 4.x（原子化样式），生态完善、文档丰富，开发效率与运行性能兼顾。
- **功能模块化**：核心功能按模块拆分（聊天、动态、路由、UI 组件等），采用「主进程-渲染进程分离」+「React 组件化」架构，低耦合易扩展，支持按需集成/删除功能。
- **工程化规范**：内置 ESLint + Prettier 代码校验、Husky 提交钩子（强制 Commit 规范）、一键打包脚本，标准化开发流程，降低团队协作成本。
- **易二次开发**：清晰的目录结构、详细的技术注释、完整的开发文档，新手可快速上手，开发者可基于现有架构扩展新功能（如音视频通话、文件传输）。

### 3. 技术选型深度解读
| 技术栈       | 选型理由                                                                 | 核心技术点/优化策略                                                                 |
|--------------|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Electron     | 无需跨平台重写代码，可复用 Web 技术栈；支持桌面端原生能力（窗口、托盘、文件操作）。 | 1. 主进程与渲染进程通过 IPC 通信，避免直接操作 DOM；2. 启用 `contextIsolation: true` 提升安全性；3. 预加载脚本（preload.js）隔离原生 API 调用。 |
| React 18     | 组件化开发效率高，Hooks 语法简化状态管理，Concurrent Mode 提升界面响应速度。   | 1. 采用函数组件 + Hooks 替代 Class 组件；2. 全局状态用 Context + useReducer 管理（轻量场景无需 Redux）；3. 利用 useMemo/useCallback 优化渲染性能。 |
| React Router 6 | 路由配置简洁，支持嵌套路由、路由守卫，适配多页面应用场景。                   | 1. 采用 `createBrowserRouter` 配置路由，支持动态路由参数；2. 路由守卫拦截未登录访问；3. 嵌套路由实现页面布局复用（如 Header/Sidebar 全局共享）。 |
| Tailwind CSS 4 | 原子化样式编写高效，无需手写复杂 CSS，支持自定义主题，适配不同设计风格需求。   | 1. 自定义主题色、字体、间距，适配产品视觉风格；2. 利用 `@apply` 封装复用样式；3. 启用 JIT 模式减少打包体积。 |
| Socket.io    | 基于 WebSocket 实现实时通信，支持断线重连、消息补发，兼容低网络环境。         | 1. 客户端断线自动重连（重试间隔 1s/3s/5s 递增）；2. 消息ACK机制确保送达；3. 命名空间（Namespace）隔离聊天/动态同步等不同场景的通信。 |
| Yarn         | 依赖安装速度快，锁文件机制确保多环境依赖一致性，避免“依赖地狱”。             | 1. 采用 Yarn Classic（1.x）保证兼容性；2. 配置镜像加速依赖下载；3. `resolutions` 字段强制锁定依赖版本（避免版本冲突）。 |

### 4. 适用场景
- 个人/小团队内部即时沟通（无需依赖第三方聊天工具，可私有化部署服务端）。
- 桌面端聊天应用二次开发（如添加行业专属功能、定制 UI 主题、集成业务系统）。
- Electron + React 技术栈学习（涵盖主进程/渲染进程通信、实时通信、多端同步、桌面端原生能力调用等核心场景）。
- 轻量社交产品原型开发（快速验证动态分享、好友互动等功能逻辑，降低试错成本）。

### 5. 开源范围说明（重要）
#### 🔥 开源内容
- Electron 主进程基础配置（窗口管理、IPC 通信、生命周期、托盘/菜单）。
- React 渲染进程界面（登录页、首页、聊天页、动态分享页等所有 UI 组件）。
- 非加密相关工具函数（日期格式化、字符串处理、组件工具、本地存储封装等）。
- 路由配置、UI 组件库（按钮、输入框、列表等基础组件，支持自定义样式）。
- 实时通信基础逻辑（Socket.io 连接、消息发送/接收、状态同步、断线重连）。
- 多端同步基础机制（数据同步触发条件、同步状态管理、离线缓存策略）。
- 工程化配置（ESLint/Prettier/Husky 规则、打包脚本、开发环境配置）。
- 配套文档（开发指南、贡献指南、FAQ、Issue/PR 模板、LICENSE）。

#### ❌ 非开源内容
- 端到端加密算法实现（如数据加密/解密逻辑、密钥生成与交换规则）。
- 密钥存储与传输安全策略（如私钥本地加密存储方案、公钥签名验证机制）。
- 服务端核心逻辑（仅提供客户端与服务端交互的接口规范，不包含服务端代码）。
- 安全防护相关代码（如防注入、防篡改、身份验证核心逻辑、异常监控）。

### 6. 版本迭代计划
| 版本   | 核心更新内容                                                                 | 技术难点                                                                 | 预计迭代时间 |
|--------|------------------------------------------------------------------------------|--------------------------------------------------------------------------|--------------|
| v1.0.0 | 基础聊天（文本消息）、动态发布/查看、多端同步（基础版）、Windows/macOS 适配。 | 1. Electron 跨平台窗口适配；2. Socket.io 实时通信稳定性。                 | 已发布       |
| v1.1.0 | 新增文件传输（图片/文档）、消息撤回功能、Linux 深度适配、快捷键支持。         | 1. Electron 文件选择与二进制流传输；2. Linux 系统依赖与权限适配。         | 待定         |
| v1.2.0 | 新增 UI 主题切换（浅色/深色）、动态点赞/评论、消息编辑、聊天记录搜索。         | 1. Tailwind CSS 主题切换与状态持久化；2. 多端同步冲突解决（如同时编辑动态）。 | 待定         |
| v2.0.0 | 支持音视频通话（基础版）、好友分组、聊天记录导出、自定义表情包。               | 1. WebRTC 音视频流传输与适配；2. 大文件分片上传与断点续传。               | 待定         |

---

## 二、快速开始
### 1. 环境要求（严格遵循，避免兼容性问题）
| 依赖工具   | 版本要求       | 推荐版本       | 验证命令                  | 下载地址                                                                 |
|------------|----------------|----------------|---------------------------|--------------------------------------------------------------------------|
| Node.js    | ≥ 16.x 且 < 21.x | 18.18.2（LTS） | `node -v`（输出版本号）   | [Node.js 官网](https://nodejs.org/zh-cn/download/releases/)               |
| Yarn       | ≥ 1.22.x 且 < 2.x | 1.22.21        | `yarn -v`（输出版本号）   | [Yarn 官网](https://classic.yarnpkg.com/zh-Hans/docs/install)             |
| Git        | ≥ 2.30.x       | 2.43.0         | `git --version`           | [Git 官网](https://git-scm.com/downloads)                                 |
| 操作系统   | Windows 10+/macOS 12+/Linux（64位） | -              | -                         | -                                                                        |
| 磁盘空间   | ≥ 2GB（含依赖+编译产物） | -              | -                         | -                                                                        |
| 网络要求   | 可访问 GitHub/ npm 镜像源 | -              | -                         | -                                                                        |

### 2. 环境安装步骤（Windows 为例，其他系统见补充说明）
#### （1）安装 Node.js
1. 从上述下载地址选择 Node.js 18.18.2（LTS）的 Windows 64 位安装包（.msi 格式）。
2. 双击安装包，勾选“Add to PATH”（自动配置环境变量），其余默认下一步完成安装（建议安装路径不包含中文/空格）。
3. 打开 PowerShell，执行 `node -v`，若输出 `v18.18.2` 则安装成功。

#### （2）安装 Yarn
1. 打开 PowerShell，执行以下命令（全局安装 Yarn 经典版，避免 2.x 版本兼容性问题）：
   ```bash
   npm install -g yarn@1.22.21
   ```
2. 执行 `yarn -v`，若输出 `1.22.21` 则安装成功。

#### （3）安装 Git
1. 下载 Git 安装包，双击安装，默认勾选“Add Git to PATH”（选择“Use Git from the Windows Command Prompt”），其余默认下一步。
2. 执行 `git --version`，若输出版本号（如 `git version 2.43.0.windows.1`）则安装成功。

### 3. 项目安装与启动（核心步骤）
#### （1）克隆仓库
```bash
# 克隆开源仓库（替换为你的仓库地址）
git clone https://github.com/你的用户名/boomchat.git
# 进入项目目录
cd boomchat
```

#### （2）配置国内镜像（解决 Electron 下载失败问题）
由于 Electron 预编译二进制文件默认从 GitHub 下载，国内网络可能存在超时/404 问题，**必须配置国内镜像**，以下是两种配置方式（推荐永久配置）：
```bash
# Windows（PowerShell）：永久配置镜像（全局生效，推荐）
yarn config set electron_mirror "https://npmmirror.com/mirrors/electron/"
yarn config set electron_builder_binaries_mirror "https://npmmirror.com/mirrors/electron-builder-binaries/"

# Windows（PowerShell）：临时配置镜像（仅当前终端有效）
$env:ELECTRON_MIRROR="https://npmmirror.com/mirrors/electron/"
$env:ELECTRON_BUILDER_BINARIES_MIRROR="https://npmmirror.com/mirrors/electron-builder-binaries/"

# macOS/Linux（终端）：永久配置镜像
yarn config set electron_mirror "https://npmmirror.com/mirrors/electron/"
yarn config set electron_builder_binaries_mirror "https://npmmirror.com/mirrors/electron-builder-binaries/"
```

#### （3）安装项目依赖
```bash
# 安装所有依赖（包含开发依赖与生产依赖）
yarn install
```
- 依赖安装成功标志：终端无红色报错，`node_modules` 目录生成，`yarn.lock` 文件更新。
- 依赖安装逻辑：Yarn 会根据 `package.json` 中的依赖列表，下载对应版本的包，并通过锁文件确保多环境一致性。
- 常见问题：若安装过程中卡住/报错，参考「FAQ」部分的“依赖安装失败排查”。

#### （4）启动开发环境
```bash
# 执行启动脚本（package.json 中已配置）
yarn start
```
- 启动逻辑：`yarn start` 会执行 `electron .` 命令，启动 Electron 应用，加载 `src/main/index.js`（主进程入口），并渲染 `src/renderer/index.html`（渲染进程入口）。

#### （5）启动成功验证
1. 终端输出 `Electron app started successfully` 字样，无报错信息。
2. 自动弹出 BoomChat 应用窗口，窗口标题为“BoomChat - 桌面聊天应用”，初始尺寸 1200×800。
3. 界面显示登录页（包含账号输入框、密码输入框、登录按钮），无控制台报错（可按 `Ctrl+Shift+I` 打开开发者工具查看渲染进程日志）。
4. 主进程日志可通过 `Ctrl+Shift+Alt+I` 打开主进程开发者工具查看。

### 4. 不同系统补充说明
| 系统       | 特殊配置/注意事项                                                                 | 依赖安装补充命令                                                                 |
|------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Windows 10+ | 需关闭“设备加密”对项目目录的影响；若启动后无窗口，检查是否被杀毒软件拦截。         | 无需额外命令，按上述步骤操作即可。                                               |
| macOS 12+  | 首次启动可能提示“无法打开来自未识别开发者的应用”，需在「系统设置→安全性与隐私」中允许打开。 | 安装 Xcode 命令行工具：`xcode-select --install`（用于编译原生依赖）。              |
| Linux      | 需提前安装系统依赖（用于 Electron 运行）。                                         | Ubuntu/Debian：`sudo apt install libgtk-3-0 libnss3 libasound2 libxss1 libxtst6`；CentOS/RHEL：`sudo dnf install gtk3 nss alsa-lib xorg-x11-server-utils`。 |

---

## 三、功能模块说明
### 1. 已实现功能（v1.0.0）
#### （1）基础窗口功能
| 功能点         | 描述                                                                 | 操作方式                                  | 技术实现细节                                                                 |
|----------------|----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------------------------------------|
| 窗口控制       | 支持最小化、最大化/还原、关闭，窗口大小可拖拽调整（最小尺寸：800×600）。 | 点击窗口标题栏按钮，或拖拽标题栏调整大小。 | 基于 Electron 的 `BrowserWindow` 类配置，通过 `minWidth`/`minHeight` 限制最小尺寸，`resizable: true` 允许拖拽调整。 |
| 托盘图标       | 应用最小化后显示在系统托盘，支持右键菜单（“显示窗口”“退出应用”）。       | 点击托盘图标显示窗口，右键呼出菜单。       | 利用 Electron 的 `Tray` 和 `Menu` 类，监听 `window-all-closed` 事件隐藏窗口而非退出应用，托盘点击事件触发窗口显示。 |
| 窗口置顶       | 支持设置窗口置顶（聊天时不被其他窗口遮挡）。                           | 登录后在首页点击“窗口置顶”按钮切换状态。   | 通过 `BrowserWindow.setAlwaysOnTop(Boolean)` 方法实现，状态存储在 `localStorage` 中，下次启动自动恢复。 |
| 开发者工具     | 支持快捷键打开调试工具（主进程+渲染进程）。                           | 渲染进程：`Ctrl+Shift+I`；主进程：`Ctrl+Shift+Alt+I`。 | 渲染进程通过 `webContents.openDevTools()` 打开；主进程需配置 `electron --inspect=5858` 启动调试。 |

#### （2）路由与页面跳转
| 路由路径       | 对应页面       | 跳转触发条件                                                                 | 技术实现细节                                                                 |
|----------------|----------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|
| `/login`       | 登录页         | 应用启动默认跳转；未登录状态访问其他路由自动重定向。                           | React Router 6 的 `createBrowserRouter` 配置，路由守卫通过 `useEffect` 监听路由变化，结合 `localStorage` 中的登录状态判断是否重定向。 |
| `/home`        | 首页（动态页） | 登录成功后自动跳转；首页包含“动态列表”“发布动态”“聊天入口”三个核心区域。       | 嵌套路由配置，`/home` 为父路由，包含 `Header`/`Sidebar` 布局组件，子路由渲染动态列表内容。 |
| `/chat/:userId`| 聊天页         | 从首页“联系人列表”点击联系人，或从动态页点击用户头像跳转。                     | 动态路由参数 `userId` 通过 `useParams()` 获取，用于请求该用户的聊天记录与实时消息。 |
| `/profile`     | 个人资料页     | 点击首页右上角“个人头像”下拉菜单中的“个人资料”跳转。                           | 路由懒加载配置：`React.lazy(() => import('../pages/Profile'))`，减少初始加载体积。 |

#### （3）实时聊天功能
| 功能点         | 描述                                                                 | 技术实现方式                          | 技术细节解读                                                                 |
|----------------|----------------------------------------------------------------------|---------------------------------------|------------------------------------------------------------------------------|
| 文本消息发送   | 支持输入纯文本消息，按 Enter 键或点击“发送”按钮提交，消息实时显示在聊天框。 | Socket.io 客户端向服务端发送消息，服务端转发至目标用户。 | 1. 输入框绑定 `keydown` 事件，Enter 键触发发送；2. 消息发送前通过 `valid.js` 校验（非空、长度≤1000字）；3. 发送后本地先渲染（乐观更新），同时等待服务端 ACK 确认。 |
| 消息列表展示   | 按时间倒序排列消息，区分自己发送（右对齐）与对方发送（左对齐）的消息。     | React 组件渲染消息列表，监听消息数据变化自动刷新。 | 1. 消息列表数据存储在 `chatStore` 中，通过 `useContext` 全局访问；2. 利用 `useEffect` 监听消息数组变化，自动滚动到底部；3. 基于 `message.senderId === user.id` 判断消息对齐方向。 |
| 消息状态提示   | 显示消息“发送中”“已送达”状态（无“已读”状态）。                           | 基于 Socket.io 确认机制（ack）实现。   | 1. 发送消息时标记 `status: 'sending'`；2. 服务端接收后返回 ack，客户端更新为 `status: 'sent'`；3. 目标用户接收后服务端推送“送达”事件，客户端更新为 `status: 'delivered'`。 |
| 聊天记录缓存   | 本地缓存当前会话的聊天记录（关闭应用后不丢失）。                           | Electron 本地存储（localStorage 扩展）。 | 1. 利用 `localForage` 库（兼容 localStorage，支持大容量存储）；2. 缓存键格式：`chat_${userId}_history`；3. 登录后自动加载缓存的聊天记录，同时从服务端拉取最新记录合并。 |

#### （4）动态分享功能（类似“朋友圈”）
| 功能点         | 描述                                                                 | 操作流程                                  | 技术实现细节                                                                 |
|----------------|----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------------------------------------|
| 发布动态       | 支持输入纯文字动态（最多 500 字），点击“发布”按钮提交。                 | 首页点击“发布动态”按钮→输入内容→点击“确认发布”。 | 1. 输入框绑定 `onChange` 事件实时监听内容，长度超过 500 字禁用发布按钮；2. 提交时通过 API 发送至服务端，同时更新本地动态列表（乐观更新）；3. 服务端返回动态 ID 后，本地替换临时 ID。 |
| 动态列表展示   | 按发布时间倒序展示所有好友动态，包含发布者头像、昵称、发布时间、动态内容。 | 首页默认加载动态列表，下拉刷新可获取最新动态。 | 1. 动态列表数据存储在 `dynamicStore` 中，通过 `useEffect` 启动时拉取；2. 下拉刷新触发 `fetchLatestDynamics()` 方法，请求服务端最新数据并合并到本地；3. 发布时间通过 `format.js` 格式化（如“10分钟前”“今天 14:30”）。 |
| 动态详情查看   | 点击单条动态可展开查看完整内容（短内容默认全部显示）。                   | 点击动态卡片即可展开/收起。               | 基于 React 状态 `isExpanded` 控制内容显示高度，短内容（≤100字）默认展开，长内容默认折叠。 |
| 动态本地缓存   | 缓存已加载的动态数据，离线状态下可查看历史动态。                         | Electron 本地存储 + 内存缓存结合。        | 1. 内存缓存当前会话的动态列表，避免重复请求；2. 本地存储缓存历史动态（缓存时间 7 天）；3. 离线时优先加载本地缓存，在线时对比更新时间拉取最新数据。 |

#### （5）多端同步功能
| 功能点         | 描述                                                                 | 触发条件                                  | 技术实现细节                                                                 |
|----------------|----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------------------------------------|
| 账号多端登录   | 支持同一账号在多个设备（如 Windows 电脑 + macOS 电脑）同时登录。         | 输入相同账号密码登录即可，服务端验证身份。 | 服务端基于 JWT 令牌验证身份，令牌包含设备标识，支持多设备同时在线。 |
| 动态多端同步   | 设备 A 发布的动态，设备 B 登录后可实时获取（延迟≤3秒）。                 | 动态发布成功后，服务端推送至同一账号所有在线设备。 | 1. 设备 A 发布动态后，服务端通过 Socket.io 向该账号的所有在线设备推送 `new-dynamic` 事件；2. 设备 B 接收事件后，拉取最新动态并更新列表。 |
| 聊天记录同步   | 设备 A 接收的消息，设备 B 登录后可同步历史聊天记录（最近 100 条）。       | 设备 B 登录成功后，自动从服务端拉取历史记录。 | 1. 登录成功后，客户端向服务端请求 `GET /api/chat/history?limit=100`；2. 拉取的历史记录与本地缓存合并，去重后渲染。 |

### 2. 待开发功能（欢迎贡献）
| 功能模块       | 待开发子功能                                                                 | 技术实现建议                                                                 | 难度等级 |
|----------------|------------------------------------------------------------------------------|------------------------------------------------------------------------------|----------|
| 聊天功能扩展   | 1. 图片/文档发送；2. 消息撤回/编辑；3. 消息表情；4. 聊天记录搜索。             | 1. 图片/文档：Electron `dialog.showOpenDialog()` 选择文件 + FormData 上传 + 服务端存储；2. 消息撤回：服务端标记消息状态 + 客户端更新 UI；3. 表情：集成 `emoji-mart` 组件 + 表情编码转换；4. 搜索：本地缓存过滤 + 服务端模糊查询。 | 中       |
| 动态功能扩展   | 1. 动态点赞/评论；2. 动态图片上传；3. 动态删除/编辑；4. 动态隐私设置（公开/仅好友可见）。 | 1. 点赞/评论：服务端维护关联数据 + Socket.io 实时推送；2. 图片上传：集成对象存储（如 MinIO/OSS）+ 图片压缩；3. 删除/编辑：服务端接口 + 客户端状态同步；4. 隐私设置：服务端权限控制 + 数据过滤。 | 中       |
| UI 体验优化   | 1. 浅色/深色模式切换；2. 自定义主题色；3. 窗口透明度调节；4. 快捷键支持（如 Ctrl+Enter 发送消息）。 | 1. 主题切换：Tailwind CSS `darkMode: 'class'` + 全局 class 切换 + 本地存储持久化；2. 透明度调节：Electron `BrowserWindow.setOpacity(0.8~1.0)`；3. 快捷键：集成 `hotkeys-js` 库监听全局快捷键。 | 低       |
| 多端同步优化   | 1. 离线消息同步；2. 动态草稿同步；3. 多端操作冲突解决（如同时编辑一条动态）。   | 1. 离线消息：服务端消息队列缓存 + 客户端上线拉取；2. 草稿同步：本地存储 + 服务端实时同步；3. 冲突解决：基于时间戳（最后操作覆盖）或乐观锁机制。 | 高       |

---

## 四、开发指南
### 1. 项目目录结构（详细版）
```
boomchat/
├── .eslintrc.js          # ESLint 配置文件（代码语法校验规则，基于 Airbnb 规范）
├── .prettierrc.js        # Prettier 配置文件（代码格式化规则，统一代码风格）
├── .husky/               # Husky 配置（Git 提交钩子，强制 Commit 规范）
│   ├── pre-commit        # 提交前执行 ESLint 校验
│   └── commit-msg        # 验证 Commit 信息格式
├── package.json          # 项目核心配置（依赖、脚本、版本、打包配置等）
├── yarn.lock             # Yarn 依赖锁文件（确保多环境依赖版本一致）
├── tailwind.config.js    # Tailwind CSS 配置（主题、插件、自定义样式）
├── src/
│   ├── main/             # Electron 主进程（负责桌面端原生能力，不可操作 DOM）
│   │   ├── index.js      # 主进程入口文件（窗口创建、生命周期管理、IPC 初始化）
│   │   ├── ipc/          # IPC 通信配置（主进程↔渲染进程通信桥梁）
│   │   │   ├── index.js  # IPC 事件注册入口（统一导出所有 IPC 事件）
│   │   │   ├── window.js # 窗口相关 IPC 事件（最小化、最大化、置顶等）
│   │   │   └── storage.js# 本地存储相关 IPC 事件（安全存储、文件操作等）
│   │   ├── window/       # 窗口配置（封装窗口创建逻辑，便于复用）
│   │   │   ├── index.js  # 窗口创建函数（返回 BrowserWindow 实例）
│   │   │   └── config.js # 窗口默认配置（尺寸、标题、图标、webPreferences 等）
│   │   └── utils/        # 主进程工具函数（非加密相关）
│   │       ├── log.js    # 日志打印工具（区分 info/warn/error 级别）
│   │       └── path.js   # 文件路径处理工具（适配不同系统路径分隔符）
│   ├── renderer/         # 渲染进程（React 界面，负责用户交互，可操作 DOM）
│   │   ├── index.js      # 渲染进程入口文件（React 挂载到 DOM）
│   │   ├── index.html    # 渲染进程入口 HTML（Electron 加载的页面模板）
│   │   ├── components/   # 公共 UI 组件（全局复用，按功能分类）
│   │   │   ├── common/   # 基础组件（按钮、输入框、列表等，无业务逻辑）
│   │   │   │   ├── Button/      # 按钮组件（支持 default/primary/danger 类型，自定义尺寸）
│   │   │   │   ├── Input/       # 输入框组件（支持校验、占位符、禁用状态）
│   │   │   │   └── List/        # 列表组件（支持下拉刷新、上拉加载、空状态）
│   │   │   ├── layout/   # 布局组件（页面框架，全局复用）
│   │   │   │   ├── Header.jsx   # 页面头部（标题、返回按钮、用户信息）
│   │   │   │   ├── Sidebar.jsx  # 侧边栏（联系人列表、导航菜单）
│   │   │   │   └── Footer.jsx   # 页面底部（版权信息、版本号）
│   │   │   └── chat/     # 聊天相关组件（业务组件，依赖聊天逻辑）
│   │   │       ├── MessageItem.jsx # 单条消息组件（接收消息数据，渲染样式）
│   │   │       └── ChatInput.jsx   # 消息输入框组件（输入、发送逻辑）
│   │   ├── pages/        # 页面组件（对应路由，聚合业务逻辑）
│   │   │   ├── Login/    # 登录页（包含登录表单、校验逻辑、登录 API 调用）
│   │   │   ├── Home/     # 首页（动态列表、发布动态组件、侧边栏）
│   │   │   ├── Chat/     # 聊天页（消息列表、输入框、聊天状态管理）
│   │   │   └── Profile/  # 个人资料页（信息展示、编辑入口）
│   │   ├── router/       # 路由配置（React Router 6 配置，包含路由守卫）
│   │   │   ├── index.js  # 路由注册（createBrowserRouter 配置所有路由）
│   │   │   └── Guard.jsx # 路由守卫（未登录拦截、权限校验）
│   │   ├── store/        # 状态管理（React Context + useReducer，轻量替代 Redux）
│   │   │   ├── index.js  # 全局状态入口（提供 Context.Provider）
│   │   │   ├── userStore.jsx  # 用户状态（登录信息、昵称、头像、token）
│   │   │   ├── chatStore.jsx  # 聊天状态（当前会话、消息列表、发送状态）
│   │   │   └── dynamicStore.jsx # 动态状态（动态列表、发布状态、刷新状态）
│   │   ├── api/          # 接口请求封装（与服务端交互，统一处理请求/响应）
│   │   │   ├── index.js  # 请求工具封装（Axios 实例 + 请求拦截器/响应拦截器）
│   │   │   ├── user.js   # 用户相关接口（登录、获取个人资料）
│   │   │   ├── chat.js   # 聊天相关接口（发送消息、获取历史记录）
│   │   │   └── dynamic.js # 动态相关接口（发布动态、获取列表、点赞）
│   │   ├── socket/       # 实时通信（Socket.io 封装，管理连接与事件）
│   │   │   ├── index.js  # Socket 连接初始化（建立连接、注册事件、断线重连）
│   │   │   ├── chat.js   # 聊天相关 Socket 事件（接收消息、状态同步）
│   │   │   └── dynamic.js # 动态相关 Socket 事件（新动态推送、点赞通知）
│   │   └── utils/        # 渲染进程工具函数（业务无关，可复用）
│   │       ├── format.js # 格式化工具（日期、字符串、数字格式化）
│   │       ├── valid.js  # 校验工具（账号、密码、动态内容、消息长度校验）
│   │       └── storage.js # 本地存储工具（封装 localStorage/localForage，统一 API）
│   └── shared/           # 主进程与渲染进程共享工具（非加密相关，避免代码冗余）
│       ├── constants.js  # 全局常量（路由路径、接口状态码、Socket 事件名）
│       └── types.js      # TypeScript 类型定义（可选，若后续支持 TS，统一类型声明）
├── public/               # 静态资源（不参与编译，直接复制到打包产物）
│   ├── icons/            # 应用图标（窗口图标、托盘图标，适配不同系统）
│   │   ├── win/          # Windows 图标（.ico 格式，256×256）
│   │   ├── mac/          # macOS 图标（.icns 格式）
│   │   └── linux/        # Linux 图标（.png 格式，128×128）
│   └── styles/           # 全局静态样式（如重置样式、基础样式）
│       └── reset.css     # CSS 重置样式（消除浏览器默认样式差异）
├── dist/                 # 打包产物目录（执行 yarn build 后生成，git 忽略）
├── LICENSE               # 开源许可证文件（MIT License）
├── ISSUE_TEMPLATE/       # GitHub Issue 模板（bug_report.md、feature_request.md）
└── PULL_REQUEST_TEMPLATE.md # GitHub PR 模板（标准化 PR 提交信息）
```

### 2. 核心目录/文件作用说明（重点解读）
| 目录/文件                | 核心作用                                                                 | 开发注意事项                                                                 | 技术细节                                                                 |
|--------------------------|--------------------------------------------------------------------------|------------------------------------------------------------------------------|--------------------------------------------------------------------------|
| src/main/index.js        | 主进程入口，负责创建窗口、初始化 IPC 通信、监听应用生命周期（ready/close 等）。 | 不可直接操作 DOM，所有 UI 相关逻辑需通过 IPC 通知渲染进程；避免阻塞主进程（如同步 IO 操作）。 | 1. 应用启动后监听 `app.ready` 事件，调用 `window/create` 创建主窗口；2. 监听 `window-all-closed` 事件，macOS 下隐藏窗口，其他系统退出应用；3. IPC 事件通过 `ipc/main` 模块注册，与渲染进程通信。 |
| src/renderer/router      | 路由配置中心，控制页面跳转逻辑与权限拦截，基于 React Router 6 实现。         | 新增页面需先在 `router/index.js` 中注册路由，再创建对应页面组件；路由守卫需处理未登录、权限不足等场景。 | 1. 采用 `createBrowserRouter` 替代传统 `BrowserRouter`，配置更灵活；2. 路由守卫通过 `useEffect` 监听 `useLocation()` 变化，判断是否允许访问当前路由；3. 嵌套路由通过 `element: <Layout />` 实现布局复用。 |
| src/renderer/components  | 公共组件库，按“基础组件+业务组件”分类，遵循“单一职责、可复用、低耦合”原则。 | 基础组件（如 Button）需支持自定义 props（样式、事件、状态）；业务组件（如 MessageItem）需依赖 props 渲染，不直接操作全局状态。 | 1. 基础组件封装通用逻辑（如 Button 的点击事件、加载状态）；2. 业务组件聚合业务逻辑（如 MessageItem 的消息对齐、状态显示）；3. 组件样式优先使用 Tailwind CSS，避免 inline-style。 |
| src/renderer/api         | 接口请求封装，基于 Axios 实现，统一处理请求头、响应拦截、错误提示。         | 新增接口需按模块拆分（如 chat.js 存放聊天相关接口）；请求拦截器统一添加 token；响应拦截器统一处理错误（如 401 未授权、500 服务器错误）。 | 1. `api/index.js` 创建 Axios 实例，配置 baseURL、超时时间（10s）；2. 请求拦截器添加 `Authorization: Bearer ${token}` 头；3. 响应拦截器处理成功响应（返回 data）和错误响应（弹出提示、401 跳转登录）。 |
| src/renderer/socket      | 实时通信封装，基于 Socket.io 客户端实现，管理连接状态、注册事件、断线重连。 | 所有 Socket 事件需在 `socket/index.js` 中初始化，避免重复连接；断线重连需处理重试间隔、最大重试次数。 | 1. `socket/index.js` 导出 `initSocket()` 函数，登录后调用初始化连接；2. 断线重连逻辑：监听 `disconnect` 事件，1s/3s/5s 递增重试，最大重试 10 次；3. 事件注册通过 `socket.on('eventName', callback)` 实现，回调函数更新全局状态。 |
| tailwind.config.js       | Tailwind CSS 自定义配置，扩展主题、添加自定义工具类、配置插件。              | 新增主题色需在 `theme.extend.colors` 中配置；自定义工具类需在 `theme.extend.fontSize`/`spacing` 等字段中添加；修改配置后需重启开发环境生效。 | 1. 主题配置：`theme: { extend: { colors: { primary: '#165DFF' }, fontSize: { 'heading': '1.5rem' } } }`；2. 插件配置：集成 `@tailwindcss/forms` 优化表单样式；3. 模式配置：`darkMode: 'class'` 支持类切换深色模式。 |

### 3. 核心技术实现示例（代码片段）
#### （1）Electron 主进程与渲染进程 IPC 通信
**主进程（src/main/ipc/window.js）**：注册窗口控制 IPC 事件
```javascript
const { ipcMain, BrowserWindow } = require('electron');

// 窗口置顶切换
ipcMain.handle('window:toggle-top', (event) => {
  const win = BrowserWindow.fromWebContents(event.sender);
  const isAlwaysOnTop = win.isAlwaysOnTop();
  win.setAlwaysOnTop(!isAlwaysOnTop);
  return !isAlwaysOnTop; // 返回新状态给渲染进程
});

// 窗口最小化
ipcMain.on('window:minimize', (event) => {
  const win = BrowserWindow.fromWebContents(event.sender);
  win.minimize();
});
```

**渲染进程（src/renderer/components/common/WindowControls.jsx）**：调用 IPC 事件
```javascript
import { useIPC } from '../../hooks/useIPC';

const WindowControls = () => {
  const { invoke } = useIPC();

  // 切换窗口置顶
  const toggleTop = async () => {
    const isTop = await invoke('window:toggle-top');
    console.log('窗口置顶状态：', isTop);
  };

  // 窗口最小化
  const minimizeWindow = () => {
    window.ipcRenderer.send('window:minimize');
  };

  return (
    <div className="window-controls">
      <button onClick={minimizeWindow} className="control-btn">—</button>
      <button onClick={toggleTop} className="control-btn">置顶</button>
    </div>
  );
};
```

#### （2）React 状态管理（Context + useReducer）
**状态定义（src/renderer/store/chatStore.jsx）**：
```javascript
import { createContext, useReducer, useContext } from 'react';

// 初始状态
const initialState = {
  currentUserId: '', // 当前聊天对象 ID
  messages: [], // 聊天消息列表
  isSending: false, // 消息发送中状态
};

// Reducer 函数
const chatReducer = (state, action) => {
  switch (action.type) {
    case 'SET_CURRENT_USER':
      return { ...state, currentUserId: action.payload };
    case 'ADD_MESSAGE':
      return { ...state, messages: [...state.messages, action.payload] };
    case 'SET_SENDING_STATUS':
      return { ...state, isSending: action.payload };
    default:
      return state;
  }
};

// 创建 Context
const ChatContext = createContext();

// Provider 组件
export const ChatProvider = ({ children }) => {
  const [state, dispatch] = useReducer(chatReducer, initialState);
  return (
    <ChatContext.Provider value={{ chatState: state, chatDispatch: dispatch }}>
      {children}
    </ChatContext.Provider>
  );
};

// 自定义 Hook，方便组件使用
export const useChatStore = () => {
  const context = useContext(ChatContext);
  if (!context) throw new Error('useChatStore 必须在 ChatProvider 中使用');
  return context;
};
```

**组件中使用（src/renderer/pages/Chat/ChatPage.jsx）**：
```javascript
import { useChatStore } from '../../store/chatStore';

const ChatPage = () => {
  const { chatState, chatDispatch } = useChatStore();
  const { currentUserId, messages, isSending } = chatState;

  // 发送消息
  const sendMessage = async (content) => {
    chatDispatch({ type: 'SET_SENDING_STATUS', payload: true });
    try {
      const message = {
        id: Date.now().toString(),
        content,
        senderId: 'current-user-id',
        receiverId: currentUserId,
        status: 'sending',
        createTime: new Date().toISOString(),
      };
      // 本地乐观更新
      chatDispatch({ type: 'ADD_MESSAGE', payload: message });
      // 调用 API 发送消息
      await api.chat.sendMessage(message);
      // 更新消息状态为已发送
      chatDispatch({
        type: 'ADD_MESSAGE',
        payload: { ...message, status: 'sent' },
      });
    } catch (error) {
      console.error('发送消息失败：', error);
    } finally {
      chatDispatch({ type: 'SET_SENDING_STATUS', payload: false });
    }
  };

  return (
    <div className="chat-page">
      <div className="message-list">
        {messages.map((msg) => (
          <MessageItem key={msg.id} message={msg} />
        ))}
      </div>
      <ChatInput onSend={sendMessage} disabled={isSending} />
    </div>
  );
};
```

#### （3）Socket.io 实时通信封装
**Socket 初始化（src/renderer/socket/index.js）**：
```javascript
import io from 'socket.io-client';
import { getToken } from '../utils/storage';
import { SOCKET_BASE_URL } from '../../shared/constants';
import { initChatSocket } from './chat';
import { initDynamicSocket } from './dynamic';

let socket = null;
let reconnectCount = 0;
const MAX_RECONNECT_COUNT = 10;

// 初始化 Socket 连接
export const initSocket = () => {
  if (socket) return socket;

  const token = getToken();
  if (!token) throw new Error('未登录，无法初始化 Socket 连接');

  // 创建 Socket 实例
  socket = io(SOCKET_BASE_URL, {
    auth: { token },
    timeout: 10000,
    reconnection: false, // 禁用默认重连，自定义重连逻辑
  });

  // 连接成功
  socket.on('connect', () => {
    console.log('Socket 连接成功');
    reconnectCount = 0;
    // 初始化各模块 Socket 事件
    initChatSocket(socket);
    initDynamicSocket(socket);
  });

  // 连接失败
  socket.on('connect_error', (error) => {
    console.error('Socket 连接失败：', error);
    reconnect();
  });

  // 断开连接
  socket.on('disconnect', (reason) => {
    console.log('Socket 断开连接：', reason);
    if (reason !== 'io client disconnect') {
      reconnect(); // 非主动断开，触发重连
    }
  });

  return socket;
};

// 重连逻辑
const reconnect = () => {
  if (reconnectCount >= MAX_RECONNECT_COUNT) {
    console.error('Socket 重连失败，已达到最大重试次数');
    return;
  }

  reconnectCount++;
  const delay = [1000, 3000, 5000][Math.min(reconnectCount - 1, 2)]; // 1s/3s/5s 递增
  setTimeout(() => {
    console.log(`Socket 第 ${reconnectCount} 次重连...`);
    socket.connect();
  }, delay);
};

// 断开 Socket 连接
export const disconnectSocket = () => {
  if (socket) {
    socket.disconnect();
    socket = null;
  }
};

// 获取 Socket 实例
export const getSocket = () => {
  if (!socket) throw new Error('Socket 未初始化，请先调用 initSocket()');
  return socket;
};
```

### 4. 开发流程（以“新增表情发送功能”为例）
#### （1）需求分析
- 功能目标：在聊天输入框添加表情选择按钮，点击弹出表情面板，选择表情后插入到输入框，支持表情与文本混合发送。
- 依赖组件：新增 `EmojiPicker` 表情选择组件，复用现有 `ChatInput` 组件，扩展 `MessageItem` 组件支持表情渲染。
- 技术栈：`emoji-mart`（表情组件库）、React 状态管理、Socket.io 消息传输。

#### （2）开发步骤
1. **安装依赖**：
   ```bash
   yarn add emoji-mart@3.0.10 # 选择稳定版本，避免兼容性问题
   ```

2. **创建表情选择组件（src/renderer/components/chat/EmojiPicker.jsx）**：
   ```javascript
   import { useState } from 'react';
   import { Picker } from 'emoji-mart';
   import 'emoji-mart/css/emoji-mart.css';

   const EmojiPicker = ({ isVisible, onSelect, onClose }) => {
     if (!isVisible) return null;

     // 表情选择回调
     const handleEmojiSelect = (emoji) => {
       onSelect(emoji.native); // 传递表情原生字符（如 😊）
     };

     return (
       <div className="emoji-picker-container">
         <Picker
           onSelect={handleEmojiSelect}
           emojiSize={24}
           title="选择表情"
           defaultCategory="people"
         />
         <button onClick={onClose} className="emoji-picker-close">关闭</button>
       </div>
     );
   };

   export default EmojiPicker;
   ```

3. **修改聊天输入框组件（src/renderer/components/chat/ChatInput.jsx）**：
   ```javascript
   import { useState } from 'react';
   import EmojiPicker from './EmojiPicker';

   const ChatInput = ({ onSend, disabled }) => {
     const [content, setContent] = useState('');
     const [showEmojiPicker, setShowEmojiPicker] = useState(false);

     // 插入表情到输入框
     const handleEmojiSelect = (emoji) => {
       setContent(prev => prev + emoji);
     };

     // 发送消息
     const handleSend = () => {
       if (!content.trim() || disabled) return;
       onSend(content.trim());
       setContent('');
     };

     // 键盘事件：Enter 发送，Shift+Enter 换行
     const handleKeyDown = (e) => {
       if (e.key === 'Enter' && !e.shiftKey) {
         e.preventDefault();
         handleSend();
       }
     };

     return (
       <div className="chat-input-container">
         <textarea
           value={content}
           onChange={(e) => setContent(e.target.value)}
           onKeyDown={handleKeyDown}
           placeholder="输入消息..."
           disabled={disabled}
           className="chat-input"
         />
         <div className="chat-input-tools">
           <button
             onClick={() => setShowEmojiPicker(!showEmojiPicker)}
             disabled={disabled}
             className="tool-btn"
           >
             😊
           </button>
           <button onClick={handleSend} disabled={disabled} className="send-btn">
             发送
           </button>
         </div>
         <EmojiPicker
           isVisible={showEmojiPicker}
           onSelect={handleEmojiSelect}
           onClose={() => setShowEmojiPicker(false)}
         />
       </div>
     );
   };

   export default ChatInput;
   ```

4. **修改消息组件支持表情渲染（src/renderer/components/chat/MessageItem.jsx）**：
   ```javascript
   const MessageItem = ({ message }) => {
     const { content, senderId, status, createTime } = message;
     const isSelf = senderId === 'current-user-id'; // 替换为实际当前用户 ID

     // 格式化消息内容（表情会自动渲染为原生字符）
     const formatMessage = (text) => {
       // 替换换行符为 <br />
       return text.split('\n').map((line, idx) => (
         <React.Fragment key={idx}>
           {line}
           {idx < text.split('\n').length - 1 && <br />}
         </React.Fragment>
       ));
     };

     return (
       <div className={`message-item ${isSelf ? 'self-message' : 'other-message'}`}>
         <div className="message-content">{formatMessage(content)}</div>
         <div className="message-meta">
           <span className="message-time">{formatTime(createTime)}</span>
           <span className="message-status">{getStatusText(status)}</span>
         </div>
       </div>
     );
   };
   ```

5. **测试功能**：
   - 启动开发环境：`yarn start`。
   - 登录后进入聊天页，点击表情按钮，选择表情插入输入框。
   - 发送包含表情的消息，验证对方是否能正常接收并显示。
   - 测试表情与文本混合发送、换行发送等场景。

6. **代码校验与提交**：
   ```bash
   # 校验代码语法
   yarn lint
   # 提交代码（遵循 Commit 规范）
   git add .
   git commit -m "feat: 新增聊天表情选择与发送功能"
   ```

### 5. 开发规范
#### （1）代码风格规范
- **JavaScript 语法**：
  - 遵循 ES6+ 语法，优先使用箭头函数、解构赋值、模板字符串、let/const（禁止 var）。
  - 字符串统一使用单引号（`'string'`），对象/数组字面量末尾不加逗号。
  - 函数参数默认值使用 `function func(a = 1) {}` 格式，避免 `a || 1`。
- **React 规范**：
  - 组件命名：使用 PascalCase（如 `MessageItem.jsx`），文件名与组件名一致。
  - Hooks 使用：禁止在条件语句、循环中调用 Hooks；自定义 Hooks 命名以 `use` 开头（如 `useSocket`）；避免 Hooks 依赖缺失（`useEffect` 依赖数组完整）。
  - 组件拆分：单个组件代码量不超过 300 行，复杂逻辑拆分至自定义 Hooks（如 `useChatLogic.js`）或工具函数。
  - Props 定义：使用 `PropTypes` 或 TypeScript 声明组件 Props 类型，避免无类型传递。
- **样式规范**：
  - 优先使用 Tailwind CSS 原子化样式，避免手写 CSS 文件（特殊场景除外）。
  - 需自定义样式时，在 `tailwind.config.js` 中扩展工具类，或使用 `@apply` 组合样式（如 `.btn-primary { @apply bg-primary text-white px-4 py-2 rounded; }`）。
  - 样式类名使用 kebab-case（如 `message-item`），避免驼峰命名。
- **文件命名**：
  - 组件文件：PascalCase（如 `Button.jsx`、`EmojiPicker.jsx`）。
  - 工具函数/配置文件：camelCase（如 `format.js`、`valid.js`）。
  - 目录命名：kebab-case（如 `message-item/`、`emoji-picker/`）。
  - 测试文件：与被测试文件同名，后缀 `.test.jsx`（如 `Button.test.jsx`）。

#### （2）提交规范（Commit 信息格式）
提交代码时，Commit 信息需严格遵循「type: 描述」格式，type 可选值及说明如下：
| type       | 适用场景                                                                 | 示例                                  |
|------------|--------------------------------------------------------------------------|---------------------------------------|
| feat       | 新增功能（如“新增表情发送功能”“新增动态点赞功能”）                          | `feat: 新增聊天表情选择与发送功能`     |
| fix        | 修复 Bug（如“修复表情插入后输入框光标错位问题”“修复动态列表刷新失败”）        | `fix: 修复表情插入后光标错位的 Bug`   |
| docs       | 文档更新（如“补充开发指南中的表情组件说明”“修复 README 中的错别字”）          | `docs: 补充表情组件开发文档`          |
| style      | 样式调整（不影响功能，如“修改表情面板宽度”“调整按钮间距”）                  | `style: 调整表情面板宽度为 300px`     |
| refactor   | 代码重构（不新增功能、不修复 Bug，如“重构表情组件逻辑”“优化 Socket 连接代码”） | `refactor: 重构 EmojiPicker 组件`     |
| test       | 新增/修改测试用例（如“新增表情发送功能测试用例”“完善聊天记录缓存测试”）        | `test: 新增表情发送功能测试用例`      |
| chore      | 工程化配置修改（如“更新依赖版本、修改 ESLint 规则、配置打包脚本”）            | `chore: 更新 electron 版本至 38.7.0`  |
| perf       | 性能优化（如“优化动态列表渲染性能”“减少 Socket 重连次数”）                    | `perf: 优化动态列表下拉刷新性能`      |
| revert     | 回滚代码（如“回滚到上一版本，取消表情功能”）                                | `revert: 回滚 feat: 新增表情发送功能` |

**提交前检查**：
- 执行 `yarn lint` 确保无语法错误。
- Commit 信息描述简洁明了，不超过 50 字，避免模糊表述（如“修改了一些问题”）。
- 单次提交尽量聚焦一个功能/修复，避免一次提交多个不相关修改。

#### （3）分支管理规范
- **main**：稳定版分支，仅用于发布正式版本，禁止直接提交代码。所有合并需通过 PR 从 `dev` 分支合并，且经过测试验证。
- **dev**：开发分支，所有功能开发完成后合并至此分支，用于集成测试。禁止直接提交代码，仅通过 PR 合并 `feature/fix` 分支。
- **feature/xxx**：功能分支，基于 `dev` 分支创建，命名格式为 `feature/功能名称`（如 `feature/emoji-send`、`feature/dynamic-like`）。开发完成后通过 PR 合并至 `dev` 分支，合并后删除该分支。
- **fix/xxx**：Bug 修复分支，基于 `dev` 分支创建，命名格式为 `fix/问题描述`（如 `fix/cursor-position`、`fix/dynamic-refresh`）。修复完成后通过 PR 合并至 `dev` 分支，合并后删除该分支。
- **release/xxx**：发布分支，基于 `dev` 分支创建，命名格式为 `release/v版本号`（如 `release/v1.1.0`）。用于版本发布前的最终测试、Bug 修复，发布完成后合并至 `main` 和 `dev` 分支，合并后删除该分支。
- **hotfix/xxx**：紧急修复分支，基于 `main` 分支创建，命名格式为 `hotfix/问题描述`（如 `hotfix/login-failure`）。用于修复生产环境紧急 Bug，修复完成后合并至 `main` 和 `dev` 分支，合并后删除该分支。

**分支操作流程示例**：
```bash
# 1. 拉取 dev 分支最新代码
git checkout dev
git pull upstream dev

# 2. 创建功能分支
git checkout -b feature/emoji-send dev

# 3. 开发完成后提交代码
git add .
git commit -m "feat: 新增聊天表情选择与发送功能"

# 4. 同步 dev 分支最新代码（避免冲突）
git pull upstream dev

# 5. 推送分支至远程仓库
git push -u origin feature/emoji-send

# 6. 发起 PR 至 dev 分支（GitHub 网页端操作）
```

### 6. 调试技巧
#### （1）渲染进程调试
- **打开开发者工具**：启动应用后按 `Ctrl+Shift+I`（Windows/Linux）或 `Cmd+Opt+I`（macOS），默认打开渲染进程开发者工具。
- **断点调试**：
  - 在 React 组件中添加 `debugger` 语句，代码执行到此处时会暂停。
  - 在开发者工具的「Sources」面板中，找到对应组件文件（如 `src/renderer/components/chat/EmojiPicker.jsx`），点击行号设置断点。
  - 断点暂停后，可查看变量值、调用栈、逐步执行代码（Step Over/Step Into/Step Out）。
- **状态查看**：在「Console」面板中输入 `window.store.getState()` 查看全局状态（需暴露全局状态），或通过 `useContext` 相关 Hooks 查看组件状态。
- **网络请求调试**：在「Network」面板中查看 API 请求/响应、Socket.io 连接状态，可筛选请求类型（XHR/Fetch/WebSocket），查看请求头、响应体、状态码。

#### （2）主进程调试
- **打开主进程开发者工具**：按 `Ctrl+Shift+Alt+I`（Windows/Linux）或 `Cmd+Opt+Alt+I`（macOS），或在主进程代码中添加 `mainWindow.webContents.openDevTools({ mode: 'detach' })`。
- **日志打印**：使用主进程工具函数 `src/main/utils/log.js` 中的 `log.info()`/`log.warn()`/`log.error()` 打印日志，终端会输出对应信息，便于追踪主进程执行流程。
- **IPC 通信调试**：在主进程 IPC 事件处理函数中添加日志，查看事件是否触发、参数是否正确；在渲染进程中通过 `console.log` 查看 IPC 调用结果。
- **远程调试**：启动应用时添加 `--inspect=5858` 参数（`electron --inspect=5858 .`），然后在 Chrome 浏览器中访问 `chrome://inspect`，点击「Configure」添加 `localhost:5858`，即可远程调试主进程。

#### （3）常见问题调试工具
- **依赖问题**：执行 `yarn why 依赖名` 查看依赖被哪个包引入，排查版本冲突；执行 `yarn list 依赖名` 查看当前安装的依赖版本。
- **打包问题**：执行 `yarn build --verbose` 查看打包详细日志，定位报错位置；检查 `package.json` 中的 `build` 配置是否正确，尤其是 `files` 字段是否包含所有核心文件。
- **性能问题**：
  - 渲染进程性能：使用开发者工具「Performance」面板录制性能分析报告，查看长任务、渲染瓶颈。
  - 主进程性能：使用 `electron-performance` 工具分析主进程 CPU/内存占用，排查阻塞主进程的操作。
- **网络问题**：使用 `curl` 或 Postman 测试 API 接口是否正常；使用 `wscat` 测试 Socket.io 服务端是否可用（`wscat -c ws://localhost:3000`）。

---

## 五、打包与部署
### 1. 打包前准备
#### （1）环境要求
- 打包环境需与目标系统一致（如打包 Windows 应用需在 Windows 系统操作，打包 macOS 应用需在 macOS 系统操作）。
- 磁盘空间≥5GB（打包过程会生成临时文件，需预留足够空间）。
- macOS 打包需安装 Xcode 命令行工具（`xcode-select --install`）；Linux 打包需安装对应系统依赖（如 `libgtk-3-0`）。

#### （2）配置应用信息（package.json）
打开 `package.json`，确认以下核心配置（可根据需求修改），`build` 字段为 electron-builder 打包配置：
```json
{
  "name": "boomchat",
  "version": "1.0.0",
  "description": "基于 Electron + React 的跨平台桌面聊天应用",
  "main": "src/main/index.js", // 主进程入口文件（必须正确配置）
  "author": "你的名称/团队名称 <your-email@example.com>",
  "license": "MIT",
  "scripts": {
    "start": "electron .",
    "build": "electron-builder",
    "lint": "eslint . --ext .js,.jsx",
    "format": "prettier --write ."
  },
  "build": {
    "appId": "com.boomchat.desktop", // 应用唯一标识（格式：com.xxx.yyy，用于系统识别）
    "productName": "BoomChat", // 应用名称（打包后显示的名称）
    "copyright": "Copyright © 2024 ${author}", // 版权信息
    "directories": {
      "output": "dist", // 打包产物输出目录
      "buildResources": "public" // 静态资源目录（图标、样式等）
    },
    "files": [ // 打包包含的文件（确保核心文件不遗漏）
      "src/**/*",
      "public/**/*",
      "package.json",
      "yarn.lock"
    ],
    "asar": true, // 启用 asar 打包（将源码打包为单个文件，保护代码）
    "win": {
      "target": [
        {
          "target": "nsis", // Windows 打包格式（nsis 为安装包，portable 为绿色版）
          "arch": ["x64"] // 架构（仅支持 64 位，32 位需额外配置）
        }
      ],
      "icon": "public/icons/win/icon.ico", // Windows 应用图标（256×256 .ico 格式）
      "requestExecutionLevel": "user", // 安装权限（user 为普通权限，admin 为管理员权限）
      "nsis": {
        "oneClick": false, // 关闭一键安装（允许用户选择安装路径）
        "allowToChangeInstallationDirectory": true, // 允许修改安装路径
        "installerIcon": "public/icons/win/installer.ico", // 安装包图标
        "uninstallerIcon": "public/icons/win/uninstaller.ico", // 卸载程序图标
        "installerHeaderIcon": "public/icons/win/installer.ico", // 安装包头部图标
        "createDesktopShortcut": true, // 创建桌面快捷方式
        "createStartMenuShortcut": true // 创建开始菜单快捷方式
      }
    },
    "mac": {
      "target": [
        {
          "target": "dmg", // macOS 打包格式（dmg 为镜像文件，zip 为压缩包）
          "arch": ["x64", "arm64"] // 架构（Intel 芯片 x64，M 芯片 arm64）
        }
      ],
      "icon": "public/icons/mac/icon.icns", // macOS 应用图标（.icns 格式）
      "hardenedRuntime": true, // 适配 macOS 安全机制（必须启用，否则无法运行）
      "gatekeeperAssess": false, // 关闭 Gatekeeper 校验（非开发者签名时启用）
      "entitlements": "build/entitlements.mac.plist", // 权限配置文件（可选）
      "category": "public.app-category.social-networking" // 应用分类（macOS 应用商店分类）
    },
    "linux": {
      "target": ["deb", "rpm", "AppImage"], // Linux 打包格式（适配不同发行版）
      "arch": ["x64"],
      "icon": "public/icons/linux/icon.png", // Linux 应用图标（128×128 .png 格式）
      "category": "Network;Chat", // 应用分类（Linux 菜单分类）
      "desktop": {
        "Name": "BoomChat",
        "Comment": "跨平台桌面聊天应用",
        "Keywords": "chat;electron;react"
      }
    }
  }
}
```

#### （3）macOS 额外配置（可选）
若需在 macOS 上打包并分发，需创建权限配置文件 `build/entitlements.mac.plist`：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>com.apple.security.app-sandbox</key>
    <false/>
    <key>com.apple.security.network.client</key>
    <true/>
    <key>com.apple.security.network.server</key>
    <true/>
  </dict>
</plist>
```

### 2. 打包命令
#### （1）安装打包依赖（若未安装）
```bash
# electron-builder 为打包核心依赖，已配置在 devDependencies 中，yarn install 会自动安装
# 若缺失，手动安装：
yarn add electron-builder --dev
```

#### （2）执行打包命令
```bash
# 打包当前系统版本（自动识别 Windows/macOS/Linux）
yarn build
```

#### （3）自定义打包（可选，按系统拆分）
```bash
# Windows：仅打包安装包（nsis）和绿色版（portable）
yarn build --win --x64 --target nsis,portable

# macOS：仅打包 dmg 镜像（适配 Intel 和 M 芯片）
yarn build --mac --x64 --arm64 --target dmg

# Linux：仅打包 deb 格式（Ubuntu/Debian 系）
yarn build --linux --x64 --target deb

# 打包并跳过签名（macOS 非开发者账号）
yarn build --mac --x64 --arm64 --target dmg --sign false
```

#### （4）打包日志查看
打包过程中，终端会输出详细日志，包含依赖检查、资源复制、编译、打包等步骤。若打包失败，可通过日志定位问题（如缺失文件、权限不足、网络错误）。

### 3. 打包产物说明
打包成功后，`dist/` 目录会生成以下文件（按系统分类）：
| 系统       | 打包产物类型                | 文件名示例                                  | 用途                                                                 |
|------------|-----------------------------|---------------------------------------------|----------------------------------------------------------------------|
| Windows    | 安装包（nsis）              | boomchat Setup 1.0.0.exe                    | 可执行安装包，双击安装，包含桌面快捷方式、开始菜单快捷方式、卸载程序。 |
| Windows    | 绿色版（portable）          | boomchat-1.0.0-win32-x64-portable.exe       | 免安装可执行文件，直接运行，无需配置环境，适合临时使用或分发。         |
| Windows    | 绿色版文件夹                | boomchat-1.0.0-win32-x64                    | 包含所有依赖文件的文件夹，运行 `electron.exe` 即可启动应用。          |
| macOS      | 镜像文件（dmg）             | BoomChat-1.0.0.dmg                          | 镜像文件，双击挂载，拖拽应用至 `Applications` 目录完成安装。           |
| macOS      | 压缩包（zip）               | BoomChat-1.0.0-mac.zip                      | 压缩后的应用文件夹，解压后可直接运行 `Electron.app`。                 |
| Linux      | Debian 系安装包（deb）      | boomchat_1.0.0_amd6
