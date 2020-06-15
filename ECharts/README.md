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
  // 图例组件。(当有多个统计图表时，使用数组包含对象，对应展示顺序)
  // 单个 echarts 实例中可以存在多个图例组件，会方便多个图例的布局。
  legend: { 
    type: 'plain', // string，图例的类型。可选值：'plain'：普通图例。缺省就是普通图例。'scroll'：可滚动翻页的图例。当图例数量较多时可以使用。
    // 当使用 'scroll' 时，使用这些设置进行细节配置。（详细参见官方文档）。
    id: 'xxx', // string，组件 ID。默认不指定。指定则可用于在 option 或者 API 中引用组件。
    // show: true, // boolean，图形是否展示。
    // selectedMode: true, // string / boolean，图例选择的模式，控制是否可以通过点击图例改变系列的显示状态。默认开启图例选择，可以设成 false 关闭。除此之外也可以设成 'single' 或者 'multipl' 使用单选或者多选模式。
    // inactiveColor: '#ccc', // Color，图例关闭时的颜色。
    // selected: {   // Object，图例选中状态表。
    // // 选中'系列1'
    // '系列1': true,
    // // 不选中'系列2'
    // '系列2': false
    // },
    // orient: 'horizontal', // string，图例列表的布局朝向。可选：'horizontal'，'vertical'。
    // align: 'auto', // string，图例标记和文本的对齐。默认自动，根据组件的位置和 orient 决定，当组件的 left 值为 'right' 以及纵向布局（orient 为 'vertical'）的时候为右对齐，即为 'right'。可选：'auto'，'left'，'right'。
    // icon: 'circle', // string，图例项的 icon。ECharts 提供的标记类型包括 'circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin', 'arrow', 'none'。
    // 可以通过 'image://url' 设置为图片，其中 URL 为图片的链接，或者 dataURI。
    // itemGap: 10, // number，图例每项之间的间隔。横向布局时为水平间隔，纵向布局时为纵向间隔。
    // itemWidth: 20, // number，图例标记的图形宽度。
    // itemHeight: 20, // number，图例标记的图形高度。
  },
  // 提示框组件。
  tooltip: {
    show: true, // boolean，是否显示提示框组件，包括提示框浮层和 axisPointer。
    trigger: 'item', // string，触发类型。
    // 可选：'item'，数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用。
    //      'axis'，坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。
    //      'none'，什么都不触发。
    showContent: true, // boolean，是否显示提示框浮层，默认显示。
    // alwaysShowContent: true, // boolean，是否永远显示提示框内容，默认情况下在移出可触发提示框区域后 一定时间 后隐藏，设置为 true 可以保证一直显示提示框内容。
    // triggerOn:'mousemove', // string，提示框触发的条件，可选：'mousemove' 鼠标移动时触发。'click' 鼠标点击时触发。 'mousemove|click' 同时鼠标移动和点击时触发。'none' 不在 'mousemove' 或 'click' 时触发。
    // showDelay: 20, // number，浮层显示的延迟，单位为 ms，默认没有延迟，也不建议设置。在 triggerOn 为 'mousemove' 时有效。
    // hideDelay: 100, // number，浮层隐藏的延迟，单位为 ms，在 alwaysShowContent 为 true 的时候无效。
    // enterable: true, // boolean，鼠标是否可进入提示框浮层中，默认为false，如需详情内交互，如添加链接，按钮，可设置为 true。
    // renderMode: 'html', // string，浮层的渲染模式，默认以 'html 即额外的 DOM 节点展示 tooltip；此外还可以设置为 'richText' 表示以富文本的形式渲染，
    // confine: true, // boolean，是否将 tooltip 框限制在图表的区域内。
    //  appendToBody: true, // boolean，从 v4.7.0 开始支持。是否将 tooltip 的 DOM 节点添加为 HTML 的 <body> 的子节点。只有当 renderMode 为 'html' 是有意义的。
    //  transitionDuration: 0.4, // number，提示框浮层的移动动画过渡时间，单位是 s，设置为 0 的时候会紧跟着鼠标移动。
    // position: [xxx,xxx], // string | Array | Function，提示框浮层的位置，默认不设置时位置会跟随鼠标的位置。
    // formatter: '{b0}: {c0}<br />{b1}: {c1}', // string | Function，提示框浮层内容格式器，支持字符串模板和回调函数两种形式。
    // backgroundColor: 'rgba(50,50,50,0.7)', // Color，提示框浮层的背景颜色。
    // borderColor: '#333', // Color，提示框浮层的边框颜色。
    //  borderWidth: 12, // number，提示框浮层的边框宽。
    padding: [5, 10], // number，提示框浮层内边距，单位px，默认各方向内边距为5，接受数组分别设定上右下左边距。
    // textStyle: {}, // Object，提示框浮层的文本样式。
    extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);' // string，额外附加到浮层的 css 样式。如浮层添加阴影。
  },
  // 系列列表。每个系列通过 type 决定自己的图表类型
  series: [{ // 这一个整个对象决定了图表的主要类型和基本设置
    type: 'pie', // string，决定图表类型
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

