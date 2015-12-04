一个使得视图可以正确响应触摸操作的包装。当按下的时候，被包装的视图的不透明度会降低，同时会有一个底层的颜色会透过它被用户看到，从而使得视图变暗或变亮。在底层，实际上会创建一个新的视图到视图层级中，如果使用的方法不正确，有时候会导致一些不希望出现的视觉效果。譬如被包装的view的backgroundColor并没有被显式声明为一个不透明的颜色。

例子：

```javascript
renderButton: function() {
  return (
    <TouchableHighlight onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </TouchableHighlight>
  );
},
```

> **注意**：TouchableHighlight只支持一个子节点
>
> 如果你希望包含多个子组件，用一个View来包装它们。

### 属性


<div class="props">
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="touchablewithoutfeedback"></a><a href="touchablewithoutfeedback.html#props">TouchableWithoutFeedback props...</a> <a class="hash-link" href="#touchablewithoutfeedback">#</a></h4>
    </div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="activeopacity"></a>activeOpacity <span class="propType">number</span> <a class="hash-link" href="#activeopacity">#</a></h4>
        <div>
            <p>决定被包装的视图在被触摸操作激活的时候以多少不透明度显示（通常在0到1之间）。</p>
        </div>
    </div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="onhideunderlay"></a>onHideUnderlay <span class="propType">function</span> <a class="hash-link" href="#onhideunderlay">#</a></h4>
        <div>
            <p>当底层的颜色被隐藏的时候调用。</p>
        </div>
    </div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="onshowunderlay"></a>onShowUnderlay <span class="propType">function</span> <a class="hash-link" href="#onshowunderlay">#</a></h4>
        <div>
            <p>当底层的颜色被显示的时候调用。</p>
        </div>
    </div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="style"></a>style <span class="propType"><a href="view.html#style">View#style</a></span> <a class="hash-link" href="#style">#</a></h4>
    </div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="underlaycolor"></a>underlayColor <span class="propType">string</span> <a class="hash-link" href="#underlaycolor">#</a></h4>
        <div>
            <p>当触摸操作激活的时候显示出来的底层的颜色。</p>
        </div>
    </div>
</div>
