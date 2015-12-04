Interactionmanager可以将一些需要长时间进行的工作计划到任何互动/动画完成之后在进行。这使得我们的JavaScript动画可以流畅地运行。

应用程序这样可以计划一个任务在交互和动画之后完成：

```javascript
InteractionManager.runAfterInteractions(() => {
  // ...long-running synchronous task...
});
```

把这个和其他的计划函数进行对比：

* requestAnimationFrame(): 对于一段时间内负责动画的代码。
* setImmediate/setTimeout(): 稍后运行代码。注意着可能会导致动画的延迟。
* runAfterInteractions(): 稍后运行代码，不会导致动画的延迟。

触摸处理系统会把一个或多个触摸操作看做一次“交互”，然后会延迟`runAfterInteractions()`的回调函数的执行，直到所有的触摸操作都已经结束或者取消。

Interactionmanager还允许应用程序在动画开始前注册动画的“句柄”，然后在动画结束后清除它。

```javascript
var handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
```

### 方法

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="runafterinteractions"></a><span class="propType">static </span>runAfterInteractions<span class="propType">(callback: Function)</span> <a class="hash-link" href="#runafterinteractions">#</a></h4>
		<div><p>计划一个函数在所有的交互和动画完成之后运行。</p></div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="createinteractionhandle"></a><span class="propType">static </span>createInteractionHandle<span class="propType">()</span> <a class="hash-link" href="#createinteractionhandle">#</a></h4>
		<div><p>通知管理器有某个动画或者交互开始了。</p></div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="clearinteractionhandle"></a><span class="propType">static </span>clearInteractionHandle<span class="propType">(handle: Handle)</span> <a class="hash-link" href="#clearinteractionhandle">#</a></h4>
		<div><p>通知管理器有某个动画或者交互已经结束了。</p></div>
	</div>
</div>

### 属性

<div class="props">
	<div class="prop"><h4 class="propTitle"><a class="anchor" name="events"></a>Events<span class="propType">: CallExpression</span> <a class="hash-link" href="#events">#</a></h4></div>
	<div class="prop"><h4 class="propTitle"><a class="anchor" name="addlistener"></a>addListener<span class="propType">: CallExpression</span> <a class="hash-link" href="#addlistener">#</a></h4></div>
</div>
