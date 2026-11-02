# HTML / CSS Scope Naming Skill

> 一套用于原生 HTML、Bootstrap 5 和 WordPress 页面/组件的轻量命名约定。
> **目标：命名短、可读、易批量替换、尽量降低跨站 CSS 冲突。**

## 1. 使用方式（给 AI 的指令）

制作或修改 HTML 页面、Section 或独立组件时，请遵循本 README：

1. 每种需要独立管理样式的区域，分配一个 **Scope Class**：`s01`、`s02`、`s03`……；同一种组件可在同一页面使用多次。这里的 `s` 代表 *Scope*，不限定 HTML 标签类型；`<section>`、`<div>` 等均可使用。
2. 该区域内的**自定义 Class** 统一采用 `sNN-名称`，如 `s01-title`、`s01-img`。使用简短、易懂的英文名称；不必强制 BEM 或 `s-/c-/l-/u-` 分类。
3. 该区域的自定义 CSS 选择器以 `.sNN` 为作用域入口，内部样式优先写成 `.s01 .s01-title`；禁止直接写全局 `.title`、`.card`、`h2` 等规则。
4. 除外部依赖的必要类名外，不使用无前缀的自定义 Class。Bootstrap、Swiper、WordPress 自动生成/规定的类名应保留，不要批量重命名。
5. 迁移或复制组件时，如果与另一种组件编号冲突，可仅在组件代码范围内把 `s01` 批量替换为 `s08`，同步检查 HTML、CSS、JS；相同组件的多个实例可以共用一个 Scope Class。
6. Scope Class 可重复使用；仅在锚点、表单、ARIA 等确需唯一标识时添加 ID，并按下文的多实例规则处理。

## 2. 基本写法

```html
<section class="s01">
  <div class="s01-inner">
    <div class="s01-media">
      <img class="s01-img" src="about.jpg" alt="Our factory">
    </div>
    <div class="s01-content">
      <h2 class="s01-title">About Us</h2>
      <p class="s01-desc">Company introduction.</p>
      <a class="s01-btn" href="/about/">Learn More</a>
    </div>
  </div>
</section>
```

```css
.s01 {
  color: #333;
  line-height: 1.6;
  padding-block: 60px;
}

.s01,
.s01 *,
.s01 *::before,
.s01 *::after {
  box-sizing: border-box;
}

.s01 .s01-inner {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 32px;
}

.s01 .s01-img {
  display: block;
  width: 100%;
  height: auto;
}

.s01 .s01-title {
  margin: 0 0 16px;
  font-size: 32px;
  line-height: 1.25;
}

.s01 .s01-desc { margin: 0 0 20px; }

@media (max-width: 767.98px) {
  .s01 .s01-inner { grid-template-columns: 1fr; }
}
```

**规则要点：** 一个独立区域一个 Scope；内部 Class 的 `sNN-` 与根 Class 保持一致；优先使用单层、短选择器，不必写 `.s01 .s01-inner .s01-content .s01-title`。

## 3. 推荐短名称

本节是开发时直接查阅的推荐词典。以下名称均为 **`sNN-` 后面的后缀**，例如 `title` → `s01-title`、`item` → `s02-item`。表格用于快速选词，**不是强制词汇表**；优先选择准确、简短、在本区域内含义明确的名称。即使后缀与 Bootstrap、WordPress 中常见的单词相同，完整 Class 仍必须带 `sNN-` 前缀。

### 3.1 结构与布局

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `wrap` | 外层包裹 | 区域内容宽度、整体包裹 |
| `inner` | 内层容器 | 内部内容整体布局 |
| `head` | 头部区域 | 标题、副标题、说明的组合 |
| `body` | 主体区域 | 模块的主要部分 |
| `foot` | 底部区域 | 操作、备注或底部信息 |
| `content` | 内容区域 | 图文中的文字或主体内容 |
| `main` | 主要内容 | 区域内的主内容区 |
| `side` | 侧边区域 | 侧栏、辅助内容 |
| `grid` | 网格容器 | CSS Grid 布局 |
| `row` | 行容器 | 一行内容或横向排列 |
| `col` | 列容器 | 单列内容或列布局 |
| `group` | 分组容器 | 一组关联元素 |
| `box` | 盒子 | 通用内容盒，优先考虑更具体名称 |

### 3.2 文字与信息

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `title` | 标题 | Section 或组件主标题 |
| `subtitle` | 副标题 | 主标题的补充标题 |
| `label` | 标签文字 | 字段标签、辅助标识 |
| `eyebrow` | 标题上方短标语 | 主标题上方的小字 |
| `desc` | 描述 | 简介、摘要文字 |
| `text` | 普通文本 | 没有更具体语义的文本 |
| `intro` | 引言 | 模块介绍、开场内容 |
| `meta` | 附加信息 | 日期、作者、分类等 |
| `note` | 备注 | 提醒、补充说明 |
| `caption` | 图注 | 图片或媒体说明 |
| `price` | 价格 | 产品或服务价格 |
| `value` | 数值 | 统计数值、参数值 |
| `unit` | 单位 | 统计指标或规格单位 |

### 3.3 图片与媒体

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `media` | 媒体区域 | 图片、视频等整体区域 |
| `img` | 图片 | 图片元素的样式 |
| `thumb` | 缩略图 | 卡片或画廊缩略图 |
| `cover` | 封面 | 封面图片或封面区 |
| `figure` | 图文对象 | 图片及说明组合 |
| `video` | 视频 | 视频播放器区域 |
| `poster` | 视频封面 | 视频预览图片 |
| `gallery` | 图片画廊 | 多图展示容器 |
| `track` | 轨道 | 轮播或横向滚动内容 |
| `slide` | 幻灯片项 | 自定义轮播的单项内容 |

### 3.4 列表、卡片与产品

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `list` | 列表容器 | 文章、产品、导航列表 |
| `item` | 列表项 | 列表中的单个条目 |
| `card` | 卡片 | 产品、博客或服务卡片 |
| `card-img` | 卡片图片 | 卡片专用图片 |
| `card-title` | 卡片标题 | 卡片内部标题 |
| `card-desc` | 卡片描述 | 卡片内部描述 |
| `badge` | 徽标 | New、Hot 等短标识 |
| `tag` | 标签 | 分类或关键词标签 |
| `category` | 分类 | 分类名称或分类入口 |
| `specs` | 规格参数 | 产品规格信息列表 |
| `feature` | 特点 | 功能或优势条目 |
| `logo` | 标志 | 品牌、合作伙伴标志 |

### 3.5 按钮、导航与交互

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `btn` | 按钮 | 自定义按钮样式 |
| `link` | 链接 | 普通文字或跳转链接 |
| `actions` | 操作区 | 多个操作按钮组合 |
| `nav` | 导航 | 区域内部导航 |
| `menu` | 菜单 | 菜单项组合 |
| `tabs` | 标签页容器 | Tab 切换区域 |
| `tab` | 标签页项 | 单个 Tab 按钮 |
| `prev` | 上一个 | 轮播上一项按钮 |
| `next` | 下一个 | 轮播下一项按钮 |
| `dots` | 圆点导航 | 轮播分页导航 |
| `pagination` | 分页 | 列表或画廊分页 |
| `toggle` | 切换按钮 | 展开、收起、开关 |
| `panel` | 内容面板 | Tab 或手风琴内容区 |
| `close` | 关闭按钮 | 弹窗、提示条关闭 |
| `icon` | 图标 | 图标容器或元素 |

### 3.6 表单与状态

| 常见名称 | 中文含义 | 典型用途 |
| --- | --- | --- |
| `form` | 表单 | 表单整体 |
| `field` | 字段区域 | 标签与输入框组合 |
| `input` | 输入框 | 文本输入控件 |
| `select` | 下拉选择 | 选择控件 |
| `textarea` | 多行文本 | 多行输入控件 |
| `error` | 错误提示 | 字段验证错误 |
| `hint` | 输入提示 | 字段辅助说明 |
| `active` | 激活状态 | 当前选中的项目 |
| `open` | 展开状态 | 当前展开的内容 |
| `disabled` | 禁用外观 | 不可操作状态的样式 |
| `loading` | 加载状态 | 加载中的反馈 |
| `hidden` | 隐藏状态 | 需配合实际隐藏规则及无障碍处理 |
| `empty` | 空内容状态 | 无数据提示 |

**使用提醒：**

- 上表的 `row`、`col`、`card`、`btn`、`nav`、`badge` 等，也是 Bootstrap 常见的完整类名；**自定义时只能写 `s01-row`、`s01-card` 等**，除非明确需要同时使用框架类。
- `swiper`、`swiper-wrapper`、`swiper-slide`、`wp-block-*` 等属于第三方约定或自动生成的类名，**不能按词典任意改名**。
- 优先使用单个清晰后缀，如 `s01-title`；只有确实需要区分多个标题时才使用 `s01-card-title`、`s01-panel-title`。不必追求最短而牺牲辨识度。
- 后缀只描述该 Scope 内的角色；迁移时批量替换 **`s01`**，无需逐一修改后缀。

## 4. CSS 隔离边界

`.s01 .s01-title` 能降低普通类名冲突，其特异性为 `0-2-0`，比 ID 选择器更容易覆盖，但**不等于 CSS 沙箱**：宿主的全局标签规则、继承属性、内联样式、`!important`、层叠层和第三方脚本仍可能影响组件。

- **组件显式定义关键属性**：字体、行高、颜色、间距、图片尺寸、布局、按钮样式等，不要依赖宿主网站的默认值。
- **不使用全局重置**，也不默认使用 `all: initial` / `all: unset`，避免破坏原生控件与无障碍行为。
- **优先在组件根部设置基础属性**，为真正需要独立控制的内部元素声明样式。
- **不使用 `!important` 作为默认解决方案**；遇到冲突先检查开发者工具中的实际生效规则与层叠顺序。
- **可兼容 Bootstrap 网格**：可以使用官方 `.container`、`.row`、`.col-*` 等类，但这些是有意引入的外部依赖；希望完全独立迁移时，优先使用组件自身的 Grid/Flex 布局。
- **不要擅自覆盖第三方结构类**，包括 `.swiper-slide`、`.wp-block-*` 等；有需要时在根作用域内精确调整。

## 5. JavaScript 与多实例

JS 尽量先定位该作用域根节点，再在内部查询目标；可用自定义 `data-*` 属性作为交互钩子，不要依赖页面全局 `.title` 等选择器：

```html
<div class="s02" data-js="gallery">
  <button class="s02-next" data-action="next" type="button">Next</button>
</div>
```

```js
document.querySelectorAll('.s02[data-js="gallery"]').forEach((root) => {
  if (root.dataset.initialized === 'true') return;
  const next = root.querySelector('[data-action="next"]');
  if (!next) return;
  next.addEventListener('click', () => {
    // 只操作当前 Scope 实例内的内容
  });
  root.dataset.initialized = 'true';
});
```

- 同一页面可多次使用 `class="s01"`；同一种组件的实例共享 CSS，不必逐个修改编号。
- 同一 Scope 内的重复子元素（如 10 张卡片）共享 `s01-card`，无需逐个设置 ID。
- JS 遍历每一个根 Class，再在当前实例内部查询，避免跨组件操作。
- 只有锚点、表单 `for`、`aria-labelledby`、`aria-controls` 等需要唯一标识时才使用 ID，确保 ID 与引用正确对应。
- 对动态或重复初始化的 JS 组件，要防止重复绑定事件；必要时提供销毁/清理逻辑。

## 6. 迁移操作

1. 将目标 HTML、对应 CSS、必要的 JS 与本地资源一起复制到新项目。
2. 检查目标页面是否已有**不同组件**占用 `s01` 命名；如有，仅在迁移组件范围内替换 `s01` → `s08`。同一种组件重复使用时不必修改编号。禁止整站无范围替换。
3. 检查 HTML Class、CSS 选择器及 JS 查询是否同步；功能性 ID（锚点、表单、ARIA）独立确认唯一性和关联关系，不作为样式前缀。第三方规定类名不要改。
4. 在原生页面及目标 Bootstrap / WordPress 环境中检查响应式布局、样式覆盖、字体、按钮、交互和多实例表现。
5. 如果一个区域已经包含独立维护的子组件，不要将子组件自身的私有前缀与父区域前缀混合替换。

## 7. 生成代码前的快速检查

- [ ] 需要独立样式的区域是否已有 Scope 根 Class（如 `s01`）？
- [ ] 自定义 Class 是否统一采用该区域的 `sNN-` 前缀？
- [ ] 自定义 CSS 是否都被对应的 `.sNN` 限定？
- [ ] 名称是否简短可读，无无意义的多层选择器？
- [ ] 是否保留了 Bootstrap / WordPress / Swiper 必需的类名？
- [ ] 是否显式设置关键视觉属性，而非假设宿主的默认 CSS？
- [ ] 功能性 ID 是否唯一？JS 是否按实例查询并避免重复初始化？
- [ ] 更换 `sNN` 后，是否无需大量调整内部结构即可迁移？

---

**一句话调用：** “请按照本仓库 README 的 Scope Naming 规则开发，根 Class 从 `s01` 开始，所有自定义 Class 使用相同 `sNN-` 前缀，CSS 限定在对应 `.sNN` 内。”

**适用边界：** 这是个人/小团队的 HTML 页面和可迁移组件开发约定，不是 W3C 标准、CSS 框架或保证绝对样式隔离的沙箱。
