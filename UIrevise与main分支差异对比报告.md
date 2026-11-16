# UIrevise 与 main 分支差异深度对比分析报告

> **报告生成时间**：2026-09-27  
> **对比基准（Merge-Base）**：`016d104f3248f749788be1297e625e4c643ffce1`  
> **分支现状**：
> - `main` 分支在公共祖先之上推进了 **2 个 Commits**（师生管理 v1.3 原型升级与移动端自评流程打通）
> - `UIrevise` 分支在公共祖先之上推进了 **19 个 Commits**，并包含工作区最新的去 AI 塑料感与多角色工作台并集成果

---

## 一、核心变更概览与分支定位

```mermaid
gitGraph
   commit id: "016d104 (公共基准 Merge-Base)"
   branch main
   checkout main
   commit id: "898f61c (移动端评估流程)"
   commit id: "9367efc (师生管理v1.3医护分层)"
   checkout UIrevise
   commit id: "a264849 (抽屉三档尺寸标准化)"
   commit id: "33f189b (课程开发重构)"
   commit id: "b3d2421 (全局账户与消息中心)"
   commit id: "586210b (九角色与真实人员工作台)"
   commit id: "Working Tree (工作台去AI味/Dock折叠/多角色并集)"
```

| 维度 | `main` 分支 | `UIrevise` 分支 |
| :--- | :--- | :--- |
| **核心定位** | 业务原型迭代（师生管理医护分层、移动端流程打通） | **全局界面与体验重构**（设计系统规范化、九角色及人员工作台、去 AI 塑料味、抽屉弹窗标准化） |
| **角色与工作台体系** | 静态单角色工作台，缺乏真实人员多身份归属 | **完整 9 角色体系 + 真实人员入口**，独创**多角色卡片并集（Union）机制** |
| **工作台交互与视觉** | 传统常规卡片，缺少侧边消息待办联动 | **Google AI Studio 风格 Docked Utility Panel**（支持 340px 展开 ⇄ 44px 极窄静音导轨一键折叠，折叠后主网格 100% 满宽自适应） |
| **待办与消息展示** | 零散无统一待办组件 | **Linear 风格平整行流（Flat Rows）**，彻底拔除彩色竖条，采用 6px 沉静小圆点指示状态 |
| **抽屉与弹窗规格** | 尺寸随意（600/800/950/1200 等多种混杂） | **严格收敛至标准三档（480 / 720 / 880 / 1100 px）** |
| **设计系统规范** | 存在多份过时、互相冲突的草案文档 | 清理废弃草案，统一归口为规范权威文档 `docs/design.md`，规范化 Design Tokens |
| **账户与权限中心** | 简单本地配置，无动态并集计算 | **`AccountHub` 状态引擎**，支持身份切换、动态权限并集、后台配置（角色/组织/菜单按钮权限） |

---

## 二、提交历史与差异明细

### 1. `main` 分支独有的提交（共 2 个）
1. `9367efc` - **feat(师生管理): 师资管理 + 学员管理 v1.3 原型升级 + 人员类别 Tab + 医护分层**
   - 教师页面阶段二「医护分层」（子 Tab、护士画像空态、教师角色标签）。
   - 学员档案人员类别 Tab 化（医师/护士/进修/学员分组）。
   - 新增公共字典底座 `shared/data/people-registry.js`。
   - 涉及文件：`pages/学员管理/`、`pages/师资管理/`、`师生管理功能说明书.md`。
2. `898f61c` - **fix: 打通移动端评估与自评流程**
   - 移动端评估、自评表单流程打通与自动化脚本。
   - 涉及文件：`scripts/verify-mobile-assessment-flow.cjs`、`移动端/shared/tdesign-app.*`。

### 2. `UIrevise` 分支上的演进与已提交内容（共 19 个）
1. **抽屉与弹窗尺寸三档收敛**（`a264849`, `33f189b`）
   - 对课程开发、目标工作台、考评与反馈弹窗中 36+ 处抽屉弹窗进行标准化归纳（480 / 720 / 880 / 1100 px）。
2. **设计规范与冲突清理**（`bbc15c3` ~ `9f0519c`）
   - 清理了 5 份互相矛盾的历史草案（`UIrevise-01/02/03`、`drawer-width-spec.md`、`ui-1-design-baseline.md`）。
   - 统一对齐权威规范 `docs/design.md` 与 CSS Tokens。
3. **全局账户与消息中心枢纽**（`b3d2421`）
   - 实装 `shared/js/account-hub.js`，实现个人中心身份切换与未读消息联动。
4. **九大角色与真实人员工作台矩阵**（`586210b`）
   - 建立 9 种专业角色体系，生成以真实教师/医生为入口的直达页面（`柴老师.html`、`王颖雯老师.html`、`程主任.html` 等）。
   - 新增架构梳理规范文档 `docs/角色与工作台信息架构-九角色梳理-20260926.md`。

---

## 三、六大核心维度的架构与功能差异分析

### 1. 角色体系与工作台模型（Role & Personal Workbenches）
- **`main` 分支**：
  - 仅具备简单的静态角色页面，角色无法映射到医院具体老师，缺乏真实多重身份支持。
- **`UIrevise` 分支**：
  - **9 大标准角色**：住院医、规培生、带教老师、责任导师、科室教学秘书、科室主任/管理员、基地秘书/管理员、培训部管理员、系统/后台管理员。
  - **真实人员入口**：建立真实人员 HTML 入口（例如 `柴老师.html`、`王颖雯老师.html`、`王老师-授课.html`、`李医生.html`、`程主任.html` 等）。
  - **多角色并集（Union）引擎**：当一个人拥有多个角色时（如柴老师身兼带教老师、科室秘书、基地管理员），工作台**智能提取所有角色的服务卡片与待办事项的完整并集**，彻底解决身份频繁切换带来的操作成本与漏办风险。

### 2. 界面去“AI 塑料味”打磨与工作台布局（Docked Panel & Flat Rows）
- **`main` 分支**：
  - 未包含工作台右侧统一消息待办面板。
- **`UIrevise` 分支**：
  - **彻底斩断 AI 塑料味**：移除了卡片左侧粗暴刺眼的**彩色装饰竖线（`border-left: 3px solid ...`）**，取消了卡片套卡片（Card-in-card boxes）以及花哨的彩色标签药丸乱炖。
  - **Google AI Studio 风格的 Docked Utility Panel（侧边原生停靠面板）**：
    - 展开态（340px）：呈现精炼的“待办与动态”流，支持全部、待办、通知切换，支持一键已读与收起。
    - **一键收折为 44px 极窄静音导轨（Vertical Rail）**：右上角配有收折按钮 `[ ⇥ ]`，点击平滑折叠为 44px 极窄轨，仅留微型铃铛、未读红点与垂直字标。
    - **主内容大厅 100% 满宽展开**：折叠后左侧卡片大厅无缝撑满全屏，卡片网格响应式扩展为 4~5 列，界面宽阔舒朗，彻底消除页面破碎感。折叠状态在本地持久化。
  - **Linear 级平整平铺行流（Flat Rows）**：
    - 事项之间仅用 1px 微弱细线分隔；
    - 状态使用 6px 沉静小圆点表达（待办为琥珀色、通知为品牌蓝、已读隐去）；
    - 操作按钮改为轻量行内文字动作（`去处理 →`），严谨克制。

### 3. 抽屉与弹窗规格标准（Modal & Drawer Standard）
- **`main` 分支**：
  - 抽屉与弹窗宽度参差不齐（有 600px、800px、950px、1200px 甚至写死样式），缺少统一规范。
- **`UIrevise` 分支**：
  - 强制推行三档标准规范：
    - **S 档（480px / 30%）**：快捷查看、单表单轻操作。
    - **M 档（720px / 50%）**：标准业务表单、详情面板。
    - **L 档（880px / 65%）**：多步骤向导、复杂对比、双栏配置。
    - **XL 档（1100px / 80%）**：全景工作台、大型矩阵配置。
  - `shared/js/course-dev-renderer.js` 与 `pages/课程开发/` 均完成对齐。

### 4. 设计系统与规范资产（Design Tokens & Canonical Docs）
- **`main` 分支**：
  - 存在 5 份分散在不同时期的草案文档，部分关于卡片间距、字体层级的定义存在冲突。
- **`UIrevise` 分支**：
  - 删除废弃文件：`drawer-width-spec.md`、`ui-1-design-baseline.md`、`抽屉弹窗三档规范与迁移清单.md` 等。
  - 将所有基准收敛到单一真理源：`docs/design.md`。
  - 规范了 `shared/styles/design-tokens.css`、`shared/styles/sidebar.css`、`shared/styles/components.css` 中的基础变量与层级。

### 5. 后台配置与官网配置贯通（Backend Config & CMS Integration）
- **`main` 分支**：
  - 官网与后台各自独立，后台未提供角色权限和官网内容配置。
- **`UIrevise` 分支**：
  - 重构 `pages/后台配置页/`：支持角色增删改查、人员角色分配、组织岗位配置、菜单及按钮权限矩阵。
  - 建立官网（`pages/官网/`）与后台管理员的配置联动通道，信息科赵老师与王颖雯老师可直接维护前台官网内容与培训配置。

### 6. 全局公共组件与运行基础设施（Global Components & Infrastructure）
- **`main` 分支**：
  - 顶栏与侧栏仅支持旧版简单数据，无账户状态中心。
- **`UIrevise` 分支**：
  - `shared/js/account-hub.js`：提供单例账户与未读通知引擎（`AccountHub.switchRole()`、`AccountHub.messages()`、`AccountHub.markRead()`）。
  - `shared/js/global-components.js`：提供 `AppUnifiedInboxPanel`、`AppPageToolbar` 等现代组件。
  - `shared/js/module-loader.js`：提供 9 角色工作台与后台配置页的动态路由分发与依赖加载。

---

## 四、代码文件变更全景矩阵

### 1. 新增的关键文件清单
- **真实人员与角色入口**：
  - `柴老师.html`、`王颖雯老师.html`、`程主任.html`、`谭老师.html`、`李医生.html`、`王同学.html`、`小翠老师.html`、`沈老师.html`、`赵老师.html`、`陆主任.html`
  - `base-manager.html`、`continuing-manager.html`、`department-manager.html`、`resource-manager.html`
- **核心业务页面**：
  - `pages/角色工作台/page.js`、`page.css`、`page.md`（并集工作台）
  - `pages/后台配置页/page.js`、`page.css`、`page.md`（角色管理、人员分配、权限矩阵）
  - `pages/科室教学日历/page.js`、`page.css`、`page.md`
  - `pages/资源库入口/page.js`、`page.css`、`page.md`
- **公共底层与测试校验**：
  - `shared/js/account-hub.js`（全局账户中心）
  - `scripts/verify-nine-role-shell.cjs`（九角色壳层校验脚本）
  - `docs/角色与工作台信息架构-九角色梳理-20260926.md`（权威信息架构文档）

### 2. 重构与深度打磨的核心文件
- `shared/styles/components.css`：实装 Docked Utility Panel、Linear Flat Rows、折叠状态 `:has()` 响应式宽度，删除所有带颜色的竖粗线。
- `shared/js/global-components.js`：`AppUnifiedInboxPanel` 增加折叠/展开与本地存储持久化能力。
- `shared/styles/sidebar.css` & `shared/js/nav-render.js`：侧边栏与头部个人中心多角色切换器支持。
- `shared/js/module-loader.js`：将 9 类角色动态路由至 `pages/角色工作台/`，将后台配置动态映射至 `pages/后台配置页/`。
- `pages/课程开发/page.css` & `shared/js/course-dev-renderer.js`：抽屉弹窗标准化落地。

### 3. 被删除的废弃规范文件
- `docs/设计系统/design.md`（与根目录统一标准重复）
- `docs/设计系统/drawer-width-spec.md`
- `docs/设计系统/ui-1-design-baseline.md`
- `docs/设计系统/抽屉弹窗三档规范与迁移清单.md`

---

## 五、后续合并与代码同步建议（Merge Strategy）

由于 `main` 和 `UIrevise` 分支在业务层和架构层各有进展，合并时需按以下策略稳妥推进：

1. **人员类别与数据字典合并**：
   - `main` 分支新增了 `shared/data/people-registry.js`（医师/护士/进修/学员类别）。
   - `UIrevise` 分支的 `shared/js/account-hub.js` 拥有九角色人员清单与权限模型。
   - **建议**：在 `account-hub.js` 中引入 `people-registry.js` 的人员分类字典，二者并不冲突，可作为师生档案与工作台的基础元数据支撑。
2. **师生管理页面无损保留**：
   - `main` 分支在 `pages/学员管理/` 与 `pages/师资管理/` 做的人员类别 Tab 与医护分层是独立业务逻辑，合并时应完整保留。
3. **移动端流程保留**：
   - `main` 分支打通的移动端评估与自评流程（`移动端/shared/`）属于移动端独立路径，可直接 Fast-forward 合入。
4. **统一设计语言与 Token 优先以 `UIrevise` 为准**：
   - 侧边栏、工作台、抽屉弹窗三档、去 AI 味设计规范及 `docs/design.md`，应全面采纳 `UIrevise` 分支的最新成果。
