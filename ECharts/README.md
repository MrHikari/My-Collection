## ECharts

### 特性

**ECharts**，一个使用 **JavaScript** 实现的**开源可视化库**，可以流畅的运行在 *PC* 和*移动设备*上，兼容当前绝大部分浏览器`（IE8/9/10/11，Chrome，Firefox，Safari等）`，底层依赖矢量图形库 **ZRender**，提供直观，交互丰富，可高度个性化定制的数据可视化图表。

### 基本参数（公共参数）

```js
// 设置参数解读
setOption = {
  // 标题组件，包含主标题和副标题。（当一个图表组件中包含多个图表展示时，用主副标题区分）
  title: [{ // 当只需要设置一个主标题时，直接使用单个对象即可。
    // id: 'xxx', // string，组件 ID。默认不指定。指定则可用于在 option 或者 API 中引用组件。
    // show: false, // boolean，默认 true，是否显示标题组件。
    text: 'Pie label alignTo', // string，主标题文本，支持使用 \n 换行。
    // link = '', // string, 主标题文本超链接。
    // target = 'blank', // string，指定窗口打开主标题超链接。可选：'self' 当前窗口打开，'blank' 新窗口打开。
    // textStyle = { }, // object，主标题的样式，将主标题当做一个（文字块），适合的基本样式（块级样式和文字样式）都可以填写。
    // textAlign = 'auto', // string，整体（包括 text 和 subtext）的水平对齐。可选值：'auto'、'left'、'right'、'center'。
  }, {
      subtext: 'alignTo: "none" (default)', // string，副标题文本，支持使用 \n 换行。
      left: '16.67%', // title 组件， 此副标题离容器左侧的距离。
      top: '75%',// 同理，title 组件， 此副标题离容器上侧的距离。
      textAlign: 'center'
      // subtextStyle = { }, // object，副标题的样式，将副标题标题当做一个（文字块），适合的基本样式（块级样式和文字样式）
      // textVerticalAlign = 'auto', // string，整体（包括 text 和 subtext）的垂直对齐。可选值：'auto'、'top'、'bottom'、'middle'。
      // triggerEvent = false, // boolean，是否触发事件。
  }, {
      subtext: 'alignTo: "labelLine"',
      left: '50%',
      top: '75%',
      textAlign: 'center'
  }, {
      subtext: 'alignTo: "edge"',
      left: '83.33%',
      top: '75%',
      textAlign: 'center'
  }],
  series: [{
      type: 'pie',
      radius: '25%',
      center: ['50%', '50%'],
      data: data,
      animation: false,
      label: {
          position: 'outer',
          alignTo: 'none',
          bleedMargin: 5
      },
      left: 0,
      right: '66.6667%',
      top: 0,
      bottom: 0
  }, {
      type: 'pie',
      radius: '25%',
      center: ['50%', '50%'],
      data: data,
      animation: false,
      label: {
          position: 'outer',
          alignTo: 'labelLine',
          bleedMargin: 5
      },
      left: '33.3333%',
      right: '33.3333%',
      top: 0,
      bottom: 0
  }, {
      type: 'pie',
      radius: '25%',
      center: ['50%', '50%'],
      data: data,
      animation: false,
      label: {
          position: 'outer',
          alignTo: 'edge',
          margin: 20
      },
      left: '66.6667%',
      right: 0,
      top: 0,
      bottom: 0
  }]
};
```

### 饼图

#### series-pie 饼图

**饼图**主要用于表现不同类目的数据在总和中的占比。每个的弧度表示数据数量的比例。

从 **ECharts** *v4.6.0* 版本起，提供了 '**labelLine**' 与 '**edge**' 两种新的布局方式。

对于一个图表中有多个饼图的场景，可以使用 `left`、`right`、`top`、`bottom`、`width`、`height` 配置每个饼图系列的位置和视口大小。

**radius**、**label.margin** 等支持`百分比`的配置项，是相对于该配置项决定的**矩形**的**大小**的。

**注意**: **饼图**更适合表现数据相对于总数的百分比等关系。如果只是表示不同类目数据间的大小，建议使用 柱状图，一般对于微小的弧度差别相比于微小的长度差别更不敏感，或者也可以通过配置 **roseType** 显示成*南丁格尔图*，通过半径大小区分数据的大小。

