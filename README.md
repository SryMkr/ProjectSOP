# 一人公司实操经验汇总
本目录围绕一人公司日常运营、工具链路、生态使用整理全套实操心得，细分内容可查阅对应文档。

## 一、Codex 生态使用指南
### （一）基础环境与中转配置
1. Token 中转站使用 **kuaipao.ai**，登录账号：`100167339@qq.com`。
2. 账号注册完成后，参照配套文档完成配置，流程清晰可直接落地。
3. 下载安装 **CC Switch**，在软件内完成快跑中转服务配置。
4. 安装 **Codex**，采用 **API 令牌方式登录**：前往快跑官网复制令牌，填入 Codex 即可正常使用。
5. VS Code 仅作为代码查看工具，现阶段常规场景已无需手动查阅代码。

### （二）Codex 实操经验
1. 提问规范：编写**描述清晰、逻辑明确**的提示词，提升执行准确率。
2. MCP 能力：支持调用本地电脑软件；Unity 可直接安装对应 MCP 包并启用，Codex 可远程操作 Unity 编辑器。
3. 功能限制：API 令牌登录模式下，**插件、追求目标、应用快照**功能会被禁用。
4. 快捷操作：连续按两次 `Ctrl` 开启语音输入，按单次 `Ctrl` 取消语音输入。
5. 个性化：可配置全局通用提示词；若长期专注单一项目，推荐使用全局 `AGENTS.md` 统一设定规则。
6. 技能扩展：技能文件前往地址下载：https://skillsmp.com/ 按需下载对应工具技能，直接放入 Codex 技能目录即可生效。
7. 指令入口：使用 `/` 唤起功能选择菜单，统一管理各类操作指令。
8. 会话管理：闲置聊天会话可先归档，归档后再清理删除。
9. 项目隔离：可将任意文件夹设为独立工作区，目录内新建专属 `AGENT.md`，单独定义该项目的执行规则与要求。
10. 其余功能可自行探索试用。
11. Codex 可以通过 SSH 直接连接宝塔服务器。具体指令交给 AI 根据当前环境生成。
    1. 在 Mac 本机生成 SSH 密钥。
    2. 把公钥加入宝塔服务器 root 授权。
    3. 在 Mac 测试 SSH，能进入宝塔服务器终端即成功。
    4. 在宝塔服务器安装 Node 20+ 和 Codex；如 Node 版本低于 16，先升级 Node，再安装 Codex。
    5. 如果 `codex` 命令找不到，检查 npm 全局路径并建立软链接，例如先执行 `npm root -g`。
    6. 在 Codex App 添加 SSH 连接。
    7. Codex 连接状态变绿后，即可打开远程目录。
    8. 远程 Codex 登录推荐采用 API Key 方式，不依赖浏览器网页登录。
12. Codex 可以通过本地 Git SSH 连接 GitHub，用于拉取、提交和推送代码。具体指令交给 AI 根据当前环境生成。
    1. 在 Mac 本机生成 GitHub 专用 SSH 密钥。
    2. 把公钥添加到 GitHub：`Settings -> SSH and GPG keys -> New SSH key`。
    3. 在 Mac 测试 GitHub SSH，`ssh -T git@github.com` 显示认证成功即完成。
    4. 将项目 Git remote 从 HTTPS 改为 SSH，例如 `git@github.com:用户名/仓库名.git`。
    5. 如果 `git push` 提示 `Permission denied (publickey)`，检查 SSH 是否使用了正确的 GitHub 私钥。
    6. 必要时在 `~/.ssh/config` 中为 `github.com` 指定 `IdentityFile`，避免每次手动指定私钥。
    7. 连接成功后，Codex 可以在本地项目中执行 `git pull`、`git add`、`git commit`、`git push` 等操作。
    8. 推送前必须先检查 `git status` 和 diff，避免把无关文件、构建产物、密钥、数据库备份或大资源文件提交到 GitHub。
## 二、微信开发者工具平台
1. 核心用途：主要用于微信小游戏本地预览与提前测试。
2. 账号配置：优先使用微信公众平台申请的**正式 AppID**，请勿使用测试 AppID。
3. 域名校验：建议保持合法域名校验开启，若手动关闭，本地测试正常也可能在正式发布后出现域名报错。
4. 域名管理：域名的新增、删除等配置，统一在微信公众平台小程序后台操作。
5. 上传发布：项目打包上传操作，最终同步至微信公众平台。
6. 配置要求：需在 `game.json` 文件中完成地理位置相关配置。
7. 常用能力：支持项目代码分包、缓存彻底清理、真机预览调试。
8. 其他功能：现阶段暂不使用，无需额外配置。
## 三、团结引擎

### 1. 开发协作方式

1. 项目里的普通脚本逻辑基本可以交给 Codex 完成，包括 C# 脚本、Unity UI 逻辑、微信小游戏桥接逻辑、接口调用逻辑等。
2. Codex 可以通过 Unity MCP 操作当前 Unity Editor 中的场景对象、组件和 Inspector 参数，也可以结合 `unity-developer`、`unity-mcp-orchestrator` 两个技能辅助开发。
3. 团结引擎的基础操作仍然需要理解。不会的操作可以直接问 AI，让 AI 结合项目实际文件解释。

### 2. 当前项目的团结引擎与插件版本

| 模块 | 当前版本 | 主要功能 |
|---|---:|---|
| 团结编辑器 / Unity Editor | `1.6.2 / 2022.3.61t3` | 项目主编辑器，负责场景、Prefab、C#、资源管理、WebGL/小游戏构建 |
| Project Instant Game | `1.0.14` | 团结小游戏解决方案，提供微信小游戏支持和资源 Streaming 工具 |
| Auto Streaming | `1.0.0` | 资源自动流式加载模块，用于降低首包、按需下载资源 |
| WXSDK | `0.1.32` | 微信小游戏团结引擎适配 SDK，用来导出微信开发者工具可以打开的小游戏工程 |

说明：WXSDK 不是微信开发者工具。WXSDK 负责从团结项目导出微信小游戏工程；微信开发者工具负责打开这个工程，进行预览、真机调试、上传体验版和提交审核。

### 3. 打包前必须配置的内容

#### 3.1 UOS CDN 配置

打包前需要确认 UOS CDN 的配置已经可用：

- `AppID`：UOS CDN 服务的应用 ID。
- `App Secret`：UOS CDN 服务密钥，用于上传和管理云端资源。
- `Bucket`：资源桶，用来存放本项目的远程资源。
- `Badge / Release`：资源发布标识，常用于区分版本，例如 `V1`。
- `CDN 地址`：小游戏运行时实际访问资源的地址。

UOS CDN 的作用是：把 Auto Streaming 处理出来的远程资源上传到云端，让小游戏首包只保留必要内容，其余资源在运行时按需下载。
## 四、宝塔服务器管理平台
宝塔服务器主要用于统一托管与管理项目，包含前端 HTML 网站、数据库、后端代码三大核心业务。

### 1. 数据库运维
数据库更新无需复杂操作，**直接上传 SQL 文件**即可快速导入并更新数据库数据。

### 2. 后端项目运维
1. 后端代码日常更新，直接在服务器后端目录中通过 **Git 拉取更新**。
2. 后端项目必须提前配置好对应的**环境变量**，保证项目正常运行。
3. 每次后端代码更新完成后，**必须重启项目服务**，更新内容才能生效。

### 3. 域名与证书配置
1. 主域名、子域名均在**域名服务商后台创建、解析生效**，生效后即可在宝塔面板绑定使用。
2. SSL 证书日常使用**宝塔免费证书**即可满足项目上线需求。

## 五、微信公众平台 - 小程序管理板块
具体操作去WeChatMini.md阅读

# 六、UOS Remote Config 灰度 & AB 测试配置规范
## 一、目标
默认拉取 prod 线上资源；开启 UOS 配置覆盖后，根据下发策略切换至 test / gray / ABTest / rollback 资源环境。

## 二、启动链路
`game.js` 初始化 → 调用后端接口 `/remote-config/resource-badge` → 后端拉取 UOS 远程配置并返回资源标识 badge → 赋值资源域名 `DATA_CDN` → 实例化 UnityManager → 启动游戏

### 环境与资源路径映射
- 默认环境：`prod`
- 可选灰度环境：`test`、`gray`、`ABTest`、`rollback`

## 三、核心修改点
1. 后端：新增 POST 请求接口 `/remote-config/resource-badge`
2. 前端模板文件路径：`client/Assets/WX-WASM-SDK-V2/Editor/template/minigame/game.js`

### 执行顺序（顺序固定，禁止调整）

requestRemoteResourceBadge().then((badge) => {
    applyRemoteResourceBadge(badge);
    const gameManager = new UnityManager(managerConfig);
    gameManager.startGame();
});

### 接口信息：POST /remote-config/resource-badge
### 对应前端模板：client/Assets/WX-WASM-SDK-V2/Editor/template/minigame/game.js
### 打包检查
项目重新导出完成后，校验导出目录下的 game.js，文件内必须包含以下两处关键字：
remote-config/resource-badge
requestRemoteResourceBadge
### 测试检查
浏览器控制台输出两条日志，日志内的环境标识 xxx 必须完全统一：
日志行 1：[RemoteConfig] DATA_CDN = .../release_by_badge/xxx/content
日志行 2：资源包下载 url: .../release_by_badge/xxx/content/CUS/...
其中 xxx 取值范围：prod、test、gray 等环境标识。
### 注意事项
测试阶段建议同一时间仅开启一个 override 覆盖配置；
UOS 配置优先级规则：优先级数值越小，配置生效优先级越高；
接口返回的每一类 badge，都必须提前绑定完整可用的 release 资源包。

