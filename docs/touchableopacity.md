一个使得视图可以正确响应触摸操作的包装。当按下的时候，被包装的视图的不透明度会降低。这个过程并不会真正改变视图层级，大部分情况下很容易添加到应用中而不会带来一些奇怪的副作用。

例子：

```javascript
renderButton: function() {
  return (
    <TouchableOpacity onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </TouchableOpacity>
  );
},
```

###属性

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="touchablewithoutfeedback"></a><a href="touchablewithoutfeedback.html#props">TouchableWithoutFeedback props...</a> <a class="hash-link" href="#touchablewithoutfeedback">#</a></h4></div>
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="activeopacity"></a>activeOpacity <span class="propType">number</span> <a class="hash-link" href="#activeopacity">#</a></h4>
        <div>
            <p>决定被包装的视图在被触摸操作激活的时候以多少不透明度显示（通常在0到1之间）。</p>
        </div>
    </div>
</div>
