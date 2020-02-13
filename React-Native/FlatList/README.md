**原文地址**：http://www.manongjc.com/article/79976.html

&emsp;&emsp;本文章介绍 FlatList 踩坑之 onEndReached，主要包括 FlatList 踩坑之 onEndReached 使用实例、应用技巧、基本知识点总结和需要注意事项，具有一定的参考价值，需要的可以参考一下。

> 最近一个 RN 项目，有使用到 FlatList 这样一个 RN 封装的组件去做上拉加载更多功能，在 iOS 和 Android 平台上，总结了以下几个遇到的问题及解决方案

#### 1. 进入页面 onReached 开始就被触发

- **解决方案**：

```js
// 伪代码如下
<FlatList
      ...
     onEndReachedThreshold={0.5}
      ...
/>
```

当 onEndReachedThreshold 设置大于 1 时，的确进入页面就触发，设置在 0-1 之间时按正常逻辑。

#### 2. 上啦加载更多 onReached 被触发两次，造成重复请求资源，性能浪费

- **解决方案**：

```js
<FlatList
    ...
    onEndReached={() => {
    if (this.canLoadMore) {
        this.loadData(true); //
        this.canLoadMore = false;
    }
    }}
    onEndReachedThreshold={0.5}
    onMomentumScrollBegin={() => {
    this.canLoadMore = true; //初始化时调用onEndReached的loadMore
    }}
    ...
/>
```

&emsp;&emsp;这是一个官方的问题，在 github 上我们可以查到有人提了这个 issue，目前一个解决方案就是我们可以通过设置一个 flag 去控制这个问题，当第一次触发完毕之后，将这个 flag 设置为 false，避免重复去执行我们需要做的 action 操作。

#### 3. 通常情况下是先调用 onMomentumScrollBegin，然后调用 onEndReached，但是可能会存在意外情况

- **解决方案**：

```js
<FlatList
    ...
    onEndReached={() => {
    setTimeout(() => {
        if (this.canLoadMore) {
            this.loadData(true);
            this.canLoadMore = false;
        }
    }, 100)
    }}
    onEndReachedThreshold={0.5}
    onMomentumScrollBegin={() => {
    this.canLoadMore = true; //初始化时调用onEndReached的loadMore
    }}
    ...
/>
```
