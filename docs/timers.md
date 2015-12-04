定时器是一个应用程序中非常重要的部分。React Native实现了[浏览器Timer](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Timers)。

## 定时器

- setTimeout, clearTimeout
- setInterval, clearInterval
- setImmediate, clearImmediate
- requestAnimationFrame, cancelAnimationFrame

`requestAnimationFrame(fn)`和`setTimeout(fn, 0)`不同，前者会在每帧刷新之后执行一次，而后者则会尽可能快的执行（在iPhone5S上有可能每秒1000次以上）。

`setImmediate`则会在当前JavaScript执行块结束的时候执行，就在将要发送批量响应数据到原生之前。注意如果你在`setImmediate`的回调函数中又执行了`setImmediate`，它会紧接着立刻执行，而不会在调用之前等待原生代码。

`Promise`的实现就使用了`setImmediate`来执行异步调用。

## InteractionManager

原生应用感觉如此流畅的一个重要原因就是在互动和动画的过程中避免繁重的操作。在React Native里，我们目前受到限制，因为我们只有一个JavaScript执行线程。不过你可以用`InteractionManager`来确保在执行繁重工作之前所有的交互和动画都已经处理完毕。

应用可以通过以下代码来安排一个任务在交互结束之后执行：

```javascript
InteractionManager.runAfterInteractions(() => {
   // ...需要长时间同步执行的任务...
});
```

我们来把它和之前的几个任务安排方法对比一下：

- requestAnimationFrame(): 用来执行在一段时间内控制视图动画的代码
- setImmediate/setTimeout/setInterval(): 在稍后执行代码。注意这又可能会导致动画延迟。
- runAfterInteractions(): 在稍后执行代码。而且不会导致动画延迟。

触摸处理系统会把一个或多个活跃的触摸认定为'交互'，并且会将`runAfterInteractions()`的回调函数延迟执行，直到所有的触摸操作都结束或取消了。

InteractionManager还允许应用程序注册动画，在动画开始时创建一个交互“句柄”，然后在结束的时候清除它。

```javascript
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```

## TimerMixin

我们发现很多React Native应用产生致命错误（闪退）的主要原因就是因为在一个组件被释放之后，某个计时器被激活。为了解决这个问题，我们推荐使用`TimerMixin`。如果你在组件中引入`TimerMixin`，你可以把你原本的`setTimeout(fn, 500)` 变成`this.setTimeout(fn, 500)`(只需要在前面加上`this.`)，然后当你组件卸载时，事件也会被正确的清除。

这个库并没有跟着React Native一起发布。想在你的项目中使用它，你需要在你的项目文件夹下输入`npm i react-timer-mixin --save`来单独安装它。

```javascript
var TimerMixin = require('react-timer-mixin');

var Component = React.createClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(
      () => { console.log('I do not leak!'); },
      500
    );
  }
});
```

我们强烈建议您使用react-timer-mixin提供的`this.setTimeout(...)`来代替`setTimeout(...)`。这可以规避许多难以排查的BUG