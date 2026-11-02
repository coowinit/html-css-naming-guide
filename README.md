# HTML / CSS Scope Naming Skill

> 一套用于原生 HTML、Bootstrap 5 和 WordPress 页面/组件的轻量命名约定。
> **目标：命名短、可读、易批量替换、尽量降低跨站 CSS 冲突。**

## 1. 使用方式（给 AI 的指令）

制作或修改 HTML 页面、Section 或独立组件时，请遵循本 README：

1. 每个需要独立管理样式的区域，分配一个唯一的 **Scope ID**：`s01`、`s02`、`s03`……。这里的 `s` 代表 *Scope*，不限定 HTML 标签类型；`<section>`、`<div>` 等均可使用。
2. 该区域内的**自定义 Class** 统一采用 `sNN-名称`，如 `s01-title`、`s01-img`。使用简短、易懂的英文名称；不必强制 BEM 或 `s-/c-/l-/u-` 分类。
3. 该区域的自定义 CSS 选择器以 `#sNN` 为作用域入口，内部样式优先写成 `#s01 .s01-title`；禁止直接写全局 `.title`、`.card`、`h2` 等规则。
4. 除外部依赖的必要类名外，不使用无前缀的自定义 Class。Bootstrap、Swiper、WordPress 自动生成/规定的类名应保留，不要批量重命名。
5. 迁移或复制组件时，可将**该组件专属** `s01` 批量替换成目标页面尚未使用的编号（如 `s08`），同步检查 HTML、CSS、JS 中的引用。
6. 对于多实例、嵌套组件和必要的 ARIA/表单关联，按下文的例外规则处理，不能因追求统一而产生重复 ID。

## 2. 基本写法

```html
<section id="s01">
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
#s01 {
  color: #333;
  line-height: 1.6;
  padding-block: 60px;
}

#s01,
#s01 *,
#s01 *::before,
#s01 *::after {
  box-sizing: border-box;
}

#s01 .s01-inner {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 32px;
}

#s01 .s01-img {
  display: block;
  width: 100%;
  height: auto;
}

#s01 .s01-title {
  margin: 0 0 16px;
  font-size: 32px;
  line-height: 1.25;
}

#s01 .s01-desc { margin: 0 0 20px; }

@media (max-width: 767.98px) {
  #s01 .s01-inner { grid-template-columns: 1fr; }
}
```

**规则要点：** 一个独立区域一个 Scope；内部 Class 的 `sNN-` 与根 ID 保持一致；优先使用单层、短选择器，不必写 `#s01 .s01-inner .s01-content .s01-title`。

## 3. 推荐短名称

| 用途 | 推荐后缀 | 示例 |
| --- | --- | --- |
| 内部容器 | `inner` | `s01-inner` |
| 头部 | `head` | `s01-head` |
| 标题 | `title` | `s01-title` |
| 副标题 | `subtitle` | `s01-subtitle` |
| 描述 | `desc` | `s01-desc` |
| 内容区 | `content` | `s01-content` |
| 图片/媒体区 | `img` / `media` | `s01-img` / `s01-media` |
| 网格 | `grid` | `s01-grid` |
| 列表/列表项 | `list` / `item` | `s01-list` / `s01-item` |
| 卡片 | `card` | `s01-card` |
| 按钮/链接 | `btn` / `link` | `s01-btn` / `s01-link` |
| 导航 | `nav` | `s01-nav` |
| 上一个/下一个 | `prev` / `next` | `s01-prev` / `s01-next` |
| 底部 | `foot` | `s01-foot` |
| 状态 | `active` / `open` | `s01-active` / `s01-open` |

词典是**推荐用语而非封闭清单**；按实际含义增加短名称即可。`s01-title` 和 `.title`、`s01-card` 和 Bootstrap 的 `.card` 是不同的完整类名。

## 4. CSS 隔离边界

`#s01 .s01-title` 可以明显降低普通类名冲突，并在常见情况下获得较高的选择器优先级，但**不等于 CSS 沙箱**：宿主的全局标签规则、继承属性、内联样式、`!important`、层叠层和第三方脚本仍可能影响组件。

- **组件显式定义关键属性**：字体、行高、颜色、间距、图片尺寸、布局、按钮样式等，不要依赖宿主网站的默认值。
- **不使用全局重置**，也不默认使用 `all: initial` / `all: unset`，避免破坏原生控件与无障碍行为。
- **优先在组件根部设置基础属性**，为真正需要独立控制的内部元素声明样式。
- **不使用 `!important` 作为默认解决方案**；遇到冲突先检查开发者工具中的实际生效规则与层叠顺序。
- **可兼容 Bootstrap 网格**：可以使用官方 `.container`、`.row`、`.col-*` 等类，但这些是有意引入的外部依赖；希望完全独立迁移时，优先使用组件自身的 Grid/Flex 布局。
- **不要擅自覆盖第三方结构类**，包括 `.swiper-slide`、`.wp-block-*` 等；有需要时在根作用域内精确调整。

## 5. JavaScript 与多实例

JS 尽量先定位该作用域根节点，再在内部查询目标；可用自定义 `data-*` 属性作为交互钩子，不要依赖页面全局 `.title` 等选择器：

```html
<div id="s02">
  <button class="s02-next" data-action="next" type="button">Next</button>
</div>
```

```js
const root = document.getElementById('s02');
if (root) {
  const next = root.querySelector('[data-action="next"]');
  next?.addEventListener('click', () => {
    // 只操作当前 Scope 内的内容
  });
}
```

- 同一 HTML 文档中，`id="s01"` 只能出现一次。
- **同一根 Scope 内的重复子元素**（如 10 张卡片）共享 `s01-card`，无需逐个分配 ID。
- **同页重复的独立组件实例**必须使用不同根 ID（例如 `s03`、`s04`）；如果需要动态重复且共用一份 CSS/JS，则允许改用**根 Class + 相同组件前缀**的特殊模式，或使用实例 ID 与共享组件 Class 分离的模式，不要复制相同 ID。
- 必须使用 `for`、`aria-labelledby`、`aria-controls` 等 ID 关联时，为每个实例生成真正唯一且对应正确的 ID；不要误把组件 Scope ID 直接复用给内部多个控件。
- 对动态或重复初始化的 JS 组件，要防止重复绑定事件；必要时提供销毁/清理逻辑。

## 6. 迁移操作

1. 将目标 HTML、对应 CSS、必要的 JS 与本地资源一起复制到新项目。
2. 检查目标页面是否已有 `s01`；如有，**仅在要迁移的组件文件/代码范围内**替换 `s01` → `s08`（示例）。不要在整站做无范围的替换。
3. 检查 HTML 的 `id`、Class、CSS 选择器、JS 查询和 ARIA 关联是否同步；第三方库的规定类名不要改。
4. 在原生页面及目标 Bootstrap / WordPress 环境中检查响应式布局、样式覆盖、字体、按钮、交互和多实例表现。
5. 如果一个区域已经包含独立维护的子组件，不要将子组件自身的私有前缀与父区域前缀混合替换。

## 7. 生成代码前的快速检查

- [ ] 需要独立样式的区域是否已有唯一 Scope ID？
- [ ] 自定义 Class 是否统一采用该区域的 `sNN-` 前缀？
- [ ] 自定义 CSS 是否都被对应的 `#sNN` 限定？
- [ ] 名称是否简短可读，无无意义的多层选择器？
- [ ] 是否保留了 Bootstrap / WordPress / Swiper 必需的类名？
- [ ] 是否显式设置关键视觉属性，而非假设宿主的默认 CSS？
- [ ] 是否避免重复 ID、JS 全局选择与重复初始化？
- [ ] 更换 `sNN` 后，是否无需大量调整内部结构即可迁移？

---

**一句话调用：** “请按照本仓库 README 的 Scope Naming 规则开发，根 ID 从 `s01` 开始，所有自定义 Class 使用相同 `sNN-` 前缀，CSS 限定在对应 `#sNN` 内。”

**适用边界：** 这是个人/小团队的 HTML 页面和可迁移组件开发约定，不是 W3C 标准、CSS 框架或保证绝对样式隔离的沙箱。
