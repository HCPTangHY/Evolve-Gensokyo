# `tech.js` 中 `techs` 对象数据结构文档

该文档详细说明了 `src/tech.js` 文件中 `techs` 对象的结构。`techs` 对象是一个包含所有游戏中科技定义的大型对象。每个键代表一个独立的科技，其值是一个描述该科技属性和行为的对象。

## 总体结构

```javascript
const techs = {
    tech_key_1: {
        // ... 科技1的属性 ...
    },
    tech_key_2: {
        // ... 科技2的属性 ...
    },
    // ... 更多科技 ...
};
```

- `techs` 是一个 key-value 集合。
- `key` (例如 `club`, `bone_tools`) 是科技的唯一标识符。
- `value` 是一个包含下面所描述字段的对象。

## 科技对象字段详解

每个科技对象可以包含以下字段。某些字段是可选的，其存在与否取决于科技的具体功能。

| 字段名 | 数据类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| `id` | `String` | 是 | 科技在 HTML 中的 DOM ID，用于界面元素的绑定和操作。 |
| `title` | `String` 或 `Function` | 是 | 科技的显示名称。可以是静态字符串，也可以是一个返回字符串的函数，以实现动态标题（例如，根据种族特性变化）。 |
| `desc` | `String` 或 `Function` | 是 | 科技的详细描述。同样，可以是静态或动态的。 |
| `category` | `String` | 是 | 科技所属的类别，用于在科技树中进行分组和显示。例如：`agriculture`, `mining`。 |
| `era` | `String` | 是 | 科技所属的时代，决定了它在科技树的哪个主要阶段出现。例如：`primitive`, `civilized`, `discovery`。 |
| `reqs` | `Object` | 是 | 解锁该科技的前置条件。一个对象，键是前置科技的 `category`，值是该类别所需的等级。例如 `{ primitive: 1 }` 表示需要 `primitive` 类别达到 1 级。 |
| `grant` | `Array` | 是 | 研发完成后授予的科技点数。一个包含两个元素的数组：`[category, level]`。例如 `['primitive', 2]` 表示将 `primitive` 科技类别的等级提升到 2。 |
| `cost` | `Object` | 是 | 研发该科技所需的资源。键是资源名称（如 `Lumber`, `Stone`, `Knowledge`），值是所需数量，可以是一个数字或一个返回数字的函数以实现动态成本。 |
| `action` | `Function` | 是 | 点击研发按钮时执行的核心逻辑。通常包含支付成本的检查 (`payCosts`) 和成功研发后触发的游戏状态变更（如解锁新资源、建筑等）。必须返回 `true` 表示研发成功，`false` 表示失败。 |
| `condition` | `Function` | 否 | 一个可选的函数，用于判断该科技是否可见或可研发。如果函数返回 `false`，则该科技通常不会显示在科技树中。 |
| `effect` | `String` 或 `Function` | 否 | 描述科技研发后产生的具体效果。可以是静态文本或动态生成的文本。 |
| `post` | `Function` | 否 | 在科技成功研发 (`action` 返回 `true`) 之后执行的附加函数，用于处理一些后续的界面更新或其他逻辑。 |
| `trait` | `Array` | 否 | 一个字符串数组，列出拥有这些种族特性的种族**才能**研发此科技。 |
| `not_trait` | `Array` | 否 | 一个字符串数组，列出拥有这些种族特性的种族**不能**研发此科技。 |
| `path` | `Array` | 否 | 指定该科技属于哪个游戏路径 (`standard` 或 `truepath`)。如果未定义，则对所有路径都可用。 |
| `wiki` | `Boolean` | 否 | 一个布尔值或返回布尔值的函数，用于决定该科技是否应出现在游戏的 Wiki 中。 |
| `flair` | `String` 或 `Function` | 否 | 一段风味文本，通常用于提供背景故事或幽默感，显示在科技效果下方。 |
| `no_queue` | `Function` | 否 | 一个函数，用于判断该科技是否可以被添加到研究队列中。如果返回 `true`，则不能添加。 |
