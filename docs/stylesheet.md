StyleSheet提供了一种类似CSS样式表的抽象。

创建一个样式表：

```javascript
var styles = StyleSheet.create({
  container: {
    borderRadius: 4,
    borderWidth: 0.5,
    borderColor: '#d6d7da',
  },
  title: {
    fontSize: 19,
    fontWeight: 'bold',
  },
  activeTitle: {
    color: 'red',
  },
});
```

使用一个样式表：

```javascript
<View style={styles.container}>
  <Text style={[styles.title, this.props.isActive && styles.activeTitle]} />
</View>
```

从代码质量角度：

* 从render函数中移除具体的样式内容，可以使代码更清晰易懂。
* 给样式命名也是对render函数中的原始组件的作用的一种标记。

从性能角度：

* 创建一个样式表，就可以使得我们后续更容易通过ID来引用样式，而不是都创建一个新的对象。
* 它还使得样式只会在JavaScript和原生之间传递一次，随后的过程都可以只传递一个ID（这个优化还未实现）。

### 方法


<div class="props">
	<div class="prop"><h4 class="propTitle"><a class="anchor" name="create"></a><span class="propType">static </span>create<span class="propType">(obj: {[key: string]: any})</span> <a class="hash-link" href="#create">#</a></h4></div>
</div>

