## 微信小程序

### 小程序简介

**小程序**是一种全新的连接用户与服务的方式，它可以在微信内被便捷地获取和传播，同时具有出色的使用体验。

#### 产品定位及功能介绍

**微信小程序**是一种全新的连接用户与服务的方式，它可以在微信内被便捷地获取和传播，同时具有出色的使用体验

#### 微信小程序

1. 微信拥有海量的用户，而且粘性很高，在微信中开发的产品，很容易得到用户的使用
2. 开发和推广一项app或者是公众号的费用和代价太高
3. 小程序开发适配成本低
4. 容易小规模试错，然后快速迭代
5. 跨平台

#### 其他小程序

1. 支付宝小程序
2. 百度小程序
3. QQ小程序
4. 今日头条 + 抖音小程序

---

### 环境准备

#### 注册账号

需要一个**从未**与微信有过*联系*的邮箱，激活邮箱，然后填写信息登记。

**注意**：如果在补充信息的最后一步管理员身份认证不成功，尝试重新编辑信息，或者**换一个浏览器重新编辑扫码**。

#### 获取APPID

前往微信公众平台，登录。在成功跳转后没在左侧菜单栏，找到**开发**。细读**开发**页面下的信息，保密。

#### 开发工具

打开微信官方文档，在**开发**下的**工具**分页面，依照自己的系统，下载对应的开发工具。

**注意**：微信小程序自带开发者工具，集 **开发**，**预览**，**调试**，**发布** 于一身的 *完整*环境。<br/>
但是并没有纯粹的微信小程序开发者，所以建议搭配自己熟悉的开发环境，比如VSCode，结合微信小程序编辑工具，实现编写代码。

---

### 微信开发者工具

[官方网站](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)

---

### 小程序的结构目录

**小程序框架**的目标是尽可能简单，高效的开发方式，可以使开发者在微信中开发具有原生APP的服务体验。

小程序框架提供了视图层语言：**WXML**，**WXSS**，逻辑部分使用 *JavaScript*，并且在视图层和逻辑层间提供了数据传输和事件系统，让开发者能够专注于数据和逻辑。

#### 小程序文件结构和传统web结构对比

|结构|传统web|微信小程序|
|-|-|-|
|结构|HTML|WXML|
|样式|CSS|WXSS|
|逻辑|JavaScript|JavaScript|
|配置|无|JSON|

---

### 小程序的配置文件

#### 全局配置文件 app.json

```json
{
  "page": [
    // 全部页面路径
  ],
  "window": {
    // 全局样式配置
  },
  "tabBar": {
    // 底部菜单展示和跳转
    // 注意： tabBar.list 需至少包含 2 项
    "list": [{
      "pagePath": "pagePath", // 页面路径，点击跳转的页面地址
      "text": "text", // 标题
      "iconPath": "iconPath", // 未被选中的图标地址
      "selectedIconPath": "selectedIconPath", // 已选中的图标地址
    },
    {
      ... // 同理，第二个点击图标的设置
    }
    ]
  },
  // list 关键字之外的其他设置（参考官方文档）
  "color": "#000000", // 只支持16位进制 
  "backgroundColor": "#0078e7"
}
```

#### 页面配置文件 page.json

这里的 *page.json* 是泛指每一个页面目录下的 *json* 文件。类似于整个小程序项目的 *app.json* 配置文件。<br/>
开发者可以独立定义每个页面的一些属性，比如顶部颜色，是否支持下拉刷新等等。

**page.json**
```json
{
  "usingComponents": {}, // 自定义组件配置
  "navigationBarTitleText": "测试页面" // 页面配置的某些属性
}
```

#### sitemap.json 配置

小程序目录下的 *sitemap.json* 文件用于配置微信小程序及其页面是否允许被微信索引。

[官方文档介绍](https://developers.weixin.qq.com/miniprogram/dev/framework/sitemap.html)

---

### 模板语法

#### 数据绑定

**注意**：布尔类型数据的填充时，`”“`和`{{}}`之间不能有空格，如果存在空格会导致识别失败。

#### 数据运算

**注意**：小程序中支持 **三元运算**、**算术运算**、**逻辑判断**、**字符串运算**。支持一些复杂代码段，例如`if else`、`switch`、`do while`、`for`。

#### 列表渲染

*js*文件中的`data`内部先准备好需要循环渲染的数组或者对象数据，然后在*wxml*文件中使用`wx:for`语句进行渲染。

**注意**：
1. `wx:for="{{数组或者对象}}"`
2. `wx:for-item="循环项的名称"`
3. `wx:for-index="循环项的索引"`
4. `wx:for-key="唯一的值"` 用来提高列表的渲染性能。`wx:key="*this"` 表示该数组是一个普通的数组， `*this`表示循环项。
5. 当出现数组的嵌套循环时，绑定的熟悉和名称不要重复。
6. 默认情况下，小程序默认 `wx:for-item="循环项的名称"` `wx:for-index="循环项的索引"`，所以当只有一层循环时，可以省略不写。
7. 对象循环时，`wx:for-item="对象的属性值"` `wx:for-index="对象的键名"`，并且修改为 `wx:for-item="value"` `wx:for-index="key"`

**示例**：
```js
Page{{
  data: {
    ......
    list: [
      {
        id: 0,
        name: "名称0"
      },
      {
        id: 1,
        name: "名称1"
      },
      {
        id: 2,
        name: "名称2"
      },
    ],
    objectData: {
      attr1: "属性1",
      attr2: "属性2",
      attr3: "属性3",
    }
  },
  ......
}}
```

```wxml
<view>
  <view
    wx:key="id"
    wx:for="{{list}}"
    wx:for-item="item"
    wx:for-index="id"
  >
    索引：{{index}}
    ---
    值：{{item.name}}
  </view>
</view>
<view>
  <view
    wx:key="attr1"
    wx:for="{{objectData}}"
    wx:for-item="value"
    wx:for-index="key"
  >
    属性名：{{key}}
    ---
    属性值：{{value}}
  </view>
</view>
```

#### block 标签

`<block/>` 并不是一个组件，它仅仅是一个**包装元素**，**不会**在页面中做*任何渲染*，只接受**控制属性**。

**比如**：因为 `wx:if` 是一个*控制属性*，需要将它添加到一个*标签*上。如果要一次性判断多个组件标签，可以使用一个 `<block/>` 标签将多个组件包装起来，并在上边使用 `wx:if` 控制属性。<br/>
类似 `block` `wx:if`，也可以将 `wx:for` 用在`<block/>`标签上，以渲染一个包含**多节点**的结构块。

#### 条件渲染

1. `wx:if`，在框架中，使用 `wx:if={{condition}}` 来判断是否需要渲染该代码块。

2. `hidden`， 在框架中，使用 `hidden={{condition}}` 来判断是否需要渲染该代码块。（不常使用，普遍使用 `wx:if`）

**注意**：当标签不是频繁切换显示的时候，优先使用 `wx:if`。频繁切换显示的时候时候`hidden`。

---

### 事件绑定

小程序中的时间绑定通过 *bind* 关键字实现。比如 `bindtap`，`bindinput`，`bindchange` 等。不同的组件支持不同的事件，详细看文档。

**input示例**：

```wxml
<input type="text" bindinput="handleInput"/>
<view>
  {{displayInput}}
</view>
```

```js
Page{{
  data: {
    displayInput: '',
    ......
  },

  handleInput(e) {
    console('handle-------->');
    console('e-event-事件源------>', e);
    console('输入值-e.detail.value----->', e.detail.value);
    this.setData({ displayInput: e.detail.value})
  }
  ......
}}
```

**button示例**：

```wxml
<button bindtap=“handleClick” customData="{{100}}">按钮组件</button>
```

```js
Page{{
  data: {
    defaultNum: 0,
    num: '',
    ......
  },

  handleClick(e) {
    console('handle-------->');
    console('e-event-事件源------>', e);
    console('传入参数值-e.currentTarget.value----->', e.currentTarget.value);
    this.setData({ num: e.currentTarget.value });
    this.setData({ defaultNum: e.currentTarget.value + this.data.defaultNum })
  }
  ......
}}
```

**注意**：点击事件 *bindtap* **无法**在小程序中的 **事件内** 直接传参，需要通过**自定义属性**的方式传递参数，最终在事件源中获得参数。

