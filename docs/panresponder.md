`PanResponder`类可以将多点触摸操作协调成一个手势。它使得一个单点触摸可以接受更多的触摸操作，也可以用于识别简单的多点触摸手势。

它提供了了一个对[触摸响应系统](/docs/gesture-responder-system.html)响应器的可预测的包装。对于每一个处理器，它提供了一个新的`gestureState`对象附加在普通事件之外。

一个`gestureState`对象有如下的字段：

* `stateID` - 触摸状态的ID。在屏幕上有touch的情况下，这个ID会一直有效。
* `moveX` - 最近一次移动时的屏幕坐标
* `moveY` - 最近一次移动时的屏幕坐标
* `x0` - 当响应器产生的时候的屏幕坐标
* `y0` - 当响应器产生的时候的屏幕坐标
* `dx` - 从触摸操作开始的总累计路程
* `dy` - 从触摸操作开始的总累计路程
* `vx` - 当前的移动速度
* `vy` - 当前的移动速度
* `numberActiveTouches` - 当前在屏幕上的有效触摸数

### 基础使用

```javascript
  componentWillMount: function() {
    this._panResponder = PanResponder.create({
      // Ask to be the responder:
      onStartShouldSetPanResponder: (evt, gestureState) => true,
      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
      onMoveShouldSetPanResponder: (evt, gestureState) => true,
      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,

      onPanResponderGrant: (evt, gestureState) => {
        // The guesture has started. Show visual feedback so the user knows
        // what is happening!

        // gestureState.{x,y}0 will be set to zero now
      },
      onPanResponderMove: (evt, gestureState) => {
        // The most recent move distance is gestureState.move{X,Y}

        // The accumulated gesture distance since becoming responder is
        // gestureState.d{x,y}
      },
      onPanResponderTerminationRequest: (evt, gestureState) => true,
      onPanResponderRelease: (evt, gestureState) => {
        // The user has released all touches while this view is the
        // responder. This typically means a gesture has succeeded
      },
      onPanResponderTerminate: (evt, gestureState) => {
        // Another component has become the responder, so this gesture
        // should be cancelled
      },
      onShouldBlockNativeResponder: (evt, gestureState) => {
        // Returns whether this component should block native components from becoming the JS
        // responder. Returns true by default. Is currently only supported on android.
        return true;
      },
    });
  },

  render: function() {
    return (
      <View {...this._panResponder.panHandlers} />
    );
  },
```

### 可以工作的例子

要想看看可以直接使用的例子，可以看看[UIExplorer中的PanResponder](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/PanResponderExample.js)

### 方法

<div class="props">
    <div class="prop">
        <h4 class="propTitle"><a class="anchor" name="create"></a><span class="propType">static </span>create<span class="propType">(config: object)</span> <a class="hash-link" href="#create">#</a></h4>
        <div>
            <p>@param {object} 配置一个加强版本的所有响应器回调，不仅仅包括原本的<code>ResponderSyntheticEvent</code>，还包括<code>PanResponder</code>手势状态的回调。你只要简单的把<code>onResponder*</code>回调中的<code>Responder</code>替换为<code>PanResponder</code>。举例来说，这个<code>config</code>对象可能看起来这样：</p>
            <ul>
                <li><code>onMoveShouldSetPanResponder: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onMoveShouldSetPanResponderCapture: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onStartShouldSetPanResponder: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onStartShouldSetPanResponderCapture: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderReject: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderGrant: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderStart: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderEnd: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderRelease: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderMove: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderTerminate: (e, gestureState) =&gt; {...}</code></li>
                <li><code>onPanResponderTerminationRequest: (e, gestureState) =&gt; {...}</code></li>
                <li>
                    <p><code>onShouldBlockNativeResponder: (e, gestureState) =&gt; {...}</code></p>
                    <p>通常来说，对那些有对应的捕获事件的事件来说，我们在不火阶段更新gestureState一次，然后在冒泡阶段直接使用即可。</p>
                    <p>注意onStartShould* 回调。 他们只会在此节点冒泡/捕获的开始/结束事件中提供已经更新过的<code>gestureState</code>。一旦这个节点成为了事件的响应者，你就可以信任所有的开始/结束事件都会被手势正确处理，并且<code>gestureState</code>也会被正确更新。(numberActiveTouches)有可能没有包含所有的触摸，除非你就是触摸事件的响应者。</p>
                </li>
            </ul>
        </div>
    </div>
</div>

### 例子

```javascript
'use strict';

var React = require('react-native');
var {
  PanResponder,
  StyleSheet,
  View,
  processColor,
} = React;

var CIRCLE_SIZE = 80;
var CIRCLE_COLOR = 'blue';
var CIRCLE_HIGHLIGHT_COLOR = 'green';

var PanResponderExample = React.createClass({

  statics: {
    title: 'PanResponder Sample',
    description: 'Shows the use of PanResponder to provide basic gesture handling.',
  },

  _panResponder: {},
  _previousLeft: 0,
  _previousTop: 0,
  _circleStyles: {},
  circle: (null : ?{ setNativeProps(props: Object): void }),

  componentWillMount: function() {
    this._panResponder = PanResponder.create({
      onStartShouldSetPanResponder: this._handleStartShouldSetPanResponder,
      onMoveShouldSetPanResponder: this._handleMoveShouldSetPanResponder,
      onPanResponderGrant: this._handlePanResponderGrant,
      onPanResponderMove: this._handlePanResponderMove,
      onPanResponderRelease: this._handlePanResponderEnd,
      onPanResponderTerminate: this._handlePanResponderEnd,
    });
    this._previousLeft = 20;
    this._previousTop = 84;
    this._circleStyles = {
      style: {
        left: this._previousLeft,
        top: this._previousTop
      }
    };
  },

  componentDidMount: function() {
    this._updatePosition();
  },

  render: function() {
    return (
      <View
        style={styles.container}>
        <View
          ref={(circle) => {
            this.circle = circle;
          }}
          style={styles.circle}
          {...this._panResponder.panHandlers}
        />
      </View>
    );
  },

  _highlight: function() {
    this.circle && this.circle.setNativeProps({
      style: {
        backgroundColor: processColor(CIRCLE_HIGHLIGHT_COLOR)
      }
    });
  },

  _unHighlight: function() {
    this.circle && this.circle.setNativeProps({
      style: {
        backgroundColor: processColor(CIRCLE_COLOR)
      }
    });
  },

  _updatePosition: function() {
    this.circle && this.circle.setNativeProps(this._circleStyles);
  },

  _handleStartShouldSetPanResponder: function(e: Object, gestureState: Object): boolean {
    // Should we become active when the user presses down on the circle?
    return true;
  },

  _handleMoveShouldSetPanResponder: function(e: Object, gestureState: Object): boolean {
    // Should we become active when the user moves a touch over the circle?
    return true;
  },

  _handlePanResponderGrant: function(e: Object, gestureState: Object) {
    this._highlight();
  },
  _handlePanResponderMove: function(e: Object, gestureState: Object) {
    this._circleStyles.style.left = this._previousLeft + gestureState.dx;
    this._circleStyles.style.top = this._previousTop + gestureState.dy;
    this._updatePosition();
  },
  _handlePanResponderEnd: function(e: Object, gestureState: Object) {
    this._unHighlight();
    this._previousLeft += gestureState.dx;
    this._previousTop += gestureState.dy;
  },
});

var styles = StyleSheet.create({
  circle: {
    width: CIRCLE_SIZE,
    height: CIRCLE_SIZE,
    borderRadius: CIRCLE_SIZE / 2,
    backgroundColor: CIRCLE_COLOR,
    position: 'absolute',
    left: 0,
    top: 0,
  },
  container: {
    flex: 1,
    paddingTop: 64,
  },
});

module.exports = PanResponderExample;
```