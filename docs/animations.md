流畅、有意义的动画对于移动应用用户体验来说是非常必要的。就像React Native的大多数部分一样，动画API也还在开发中，不过我们已经可以联合使用两个互补的系统：`LayoutAnimation`用户全局的布局处理，而`Animated`则用于创建对某些值的更细小以及互动控制的动画。

### Animated

`Animated`库可以使得开发者可以非常容易地表达一些多样化的动画，以及交互表现力，并且具备极高的性能。`Animated`关注与声明输入与输出之间的关系，在其中简历一个可配置的变化函数，然后使用简单的`start/stop`方法来控制基于时间的动画执行。举个例子，一个在加载时带有简单的弹跳动画的组件完成后可能像这样：

```javascript
class Playground extends React.Component {
  constructor(props: any) {
    super(props);
    this.state = {
      bounceValue: new Animated.Value(0),
    };
  }
  render(): ReactElement {
    return (
      <Animated.Image                         // Base: Image, Text, View
        source={{uri: 'http://i.imgur.com/XMKOH81.jpg'}}
        style={{
          flex: 1,
          transform: [                        // `transform` is an ordered array
            {scale: this.state.bounceValue},  // Map `bounceValue` to `scale`
          ]
        }}
      />
    );
  }
  componentDidMount() {
    this.state.bounceValue.setValue(1.5);     // Start large
    Animated.spring(                          // Base: spring, decay, timing
      this.state.bounceValue,                 // Animate `bounceValue`
      {
        toValue: 0.8,                         // Animate to smaller size
        friction: 1,                          // Bouncier spring
      }
    ).start();                                // Start the animation
  }
}
```

`bounceValue`在构造函数时初始化为`state`的一部分，然后绑定到图片的缩放比例上。在场景背后，数值会被不断的计算并用于设置缩放比例。当组件刚刚挂载的时候，缩放比例被设置到1.5，然后紧跟着一个弹跳的动画开始在`bounceValue`上进行，这会每帧更新弹跳动画，并更新所有依赖的绑定（在这个例子里，就是图片的缩放比例）。这会以一个比调用`setState`然后重新渲染要快速的方法完成。因为整个配置都是声明式的，我们可以实现更进一步的优化，只要序列化好配置，然后我们可以在一个高优先级的线程执行动画。

#### 核心API

大部分你需要的东西都来自`Animated`模块。这包括两个值类型，`Value`用于单个的值，而`ValueXY`用于向量值；还包括三种动画类型，`spring`，`decay`，还有`timing`，以及三种组件类型，`View`，`Text`和`Image`。你可以使用`Animated.createAnimatedComponent`来让其它组件可以运动。

这三种动画类型可以用户创建几乎任何你需要的动画曲线，因为它们每一个都可以被自定义：

* `spring`: 基础的单次弹跳物理模型，符合[Origami设计标准](https://facebook.github.io/origami/)
  * `friction`: 控制“弹跳系数”、夸张系数，默认为7.
  * `tension`: 控制速度，默认40。
* `decay`: 以一个初始速度开始并且逐渐减慢停止。
  * `velocity`: 起始速度，必填参数。
  * `deceleration`: 速度衰减比例，默认为0.997。
* `timing`: 从时间范围映射到渐变的值。
  * `duration`: 动画持续的事件（单位是毫秒），默认为500。
  * `easing`：一个用于定义曲线的渐变函数。阅读`Easing`模块可以找到许多预定义的函数。iOS默认为`Easing.inOut(Easing.ease)`。
  * `delay`: 在一段时间之后开始动画（单位是毫秒），默认为0。

动画可以通过调用`start`方法来开始。`start`接受一个回调函数，当动画结束的时候会调用此回调函数。如果动画师因为正常播放完成而结束的，回调函数被调用时的参数为`{finished: true}`，不过如果动画是因为在结束之前被调用了`stop`而结束（可能是被一个手势或者其它的动画打断），它会收到参数`{finished: false}`。

#### 组合动画

多个动画可以通过`parallel`（同时执行）、`sequence`（顺序执行）、`stagger`、和`delay`来组合使用。它们中的每一个都接受一个要执行的动画数组，并且自动在适当的时候调用start/stop。举个例子：

```javascript
Animated.sequence([            // spring to start and twirl after decay finishes
  Animated.decay(position, {   // coast to a stop
    velocity: {x: gestureState.vx, y: gestureState.vy}, // velocity from gesture release
    deceleration: 0.997,
  }),
  Animated.parallel([          // after decay, in parallel:
    Animated.spring(position, {
      toValue: {x: 0, y: 0}    // return to start
    }),
    Animated.timing(twirl, {   // and twirl
      toValue: 360,
    }),
  ]),
]).start();                    // start the sequence group
```

默认情况下，如果任何一个动画被停止或中断了，组内所有其它的动画也会被停止。Parallel有一个`stopTogether`属性，如果设置为`false`，可以禁用自动停止。

#### 插值

`Animated` API还有一个很强大的部分就是`interpolate`函数。它接受一个输入范围并且映射到一个不同的输出范围。举个例子，一个简单的从0-1范围映射到0-100范围的映射这么写：

```javascript
value.interpolate({
  inputRange: [0, 1],
  outputRange: [0, 100],
});
```

`interpolate`还支持定义多个范围段落，这在定义静区和其他技巧的时候常用。举个例子，要让输入在-300附近的时候取相反值，然后在输入到-100附近的时候到达0，然后在输出到0附近的时候输出又回到1，接着一直到输入到100的过程逐步回到0，最后形成一个的静区，对于任何大于100的输入都返回0，你可以这么做：

```javascript
value.interpolate({
  inputRange: [-300, -100, 0, 100, 101],
  outputRange: [300,    0, 1,   0,   0],
});
```

它的最终映射结果如下：

Input | Output
------|-------
  -400|    450
  -300|    300
  -200|    150
  -100|      0
   -50|    0.5
     0|      1
    50|    0.5
   100|      0
   101|      0
   200|      0

`interpolation`还支持任意的渐变函数，其中有很多已经在`Easing`类中定义了，包括二次曲线、指数曲线、贝塞尔曲线等等。`interpolation`还支持限制输出范围`outputRange`。你可以通过设置`extrapolate`、`extrapolateLeft`或`extrapolateRight`属性来设置输出范围。默认值是`extend`，不过你可以使用`clamp`选项来阻止输出值超过`outputRange`。

#### 跟踪动态值

动态值还可以跟踪别的值。你只要把toValue设置成另一个动态值而不是一个普通数字就行了。举个例子我们用弹跳物理来和“聊天头像”进行交互，或者通过`timing`设置`duration:0`来实现一个僵硬瞬间的移动。他们还可以使用插值来进行组合：

```javascript
Animated.spring(follower, {toValue: leader}).start();
Animated.timing(opacity, {
  toValue: pan.x.interpolate({
    inputRange: [0, 300],
    outputRange: [1, 0],
  }),
}).start();
```

`ValueXY`是一个方便的处理2D交互的办法，譬如旋转或拖拽。它是一个简单的包含了两个`Animated.Value`实例的包装，然后提供了一系列辅助函数，使得`ValueXY`在许多时候可以替代`Value`来使用。举例来说，在上面的代码片段中，`leader`和`follower`可以同时为`valueXY`类型，这样x和y的值都会如你期望的被跟踪。

#### 输入事件。

`Animated.event`是Animated API中与输入有关的部分，允许手势或其它事件直接绑定到动态值上。这通过一个结构化的映射语法来完成，这样复杂事件对象中的值可以正确的被解开。第一层是一个数组，允许同时映射多个值，然后数组的每一个元素是一个嵌套的对象。在下面的例子里，你可以发现`scrollX`被映射到了`event.nativeEvent.contentOffset.x`(`event`通常是回调函数的第一个参数)，并且`pan.x`和`pan.y`分别映射到`gestureState.dx`和`gestureState.dy`（`gestureState`是传递给`PanResponder`回调函数的第二个参数）。

```javascript
onScroll={Animated.event(
  [{nativeEvent: {contentOffset: {x: scrollX}}}]   // scrollX = e.nativeEvent.contentOffset.x
)}
onPanResponderMove={Animated.event([
  null,                                          // ignore the native event
  {dx: pan.x, dy: pan.y}                         // extract dx and dy from gestureState
]);
```

#### 响应当前的动画值

你可能会注意到这里没有一个正常的方法来在动画的过程中读取当前的值——这是因为出于优化角度考虑，有些值值在原生代码中才知道。如果你希望在JavaScript中响应当前的值，有两种可能的办法：

* `spring.stopAnimation(callback)`会停止动画并且把最终的值作为参数传递给回调函数`callback`——这在处理手势动画的时候非常有用
* `spring.addListener(callback)` 会在动画的执行过程中持续异步调用`callback`回调函数，提供一个曾经的值作为参数。这在用于触发状态切换的时候非常有用，譬如当用户拖拽一个东西靠近的时候弹出一个新的气泡选项。不过这个状态切换可能并不会十分灵敏，因为它不像许多连续手势操作（如旋转）那样在60fps下运行。

#### 后续工作

如前面所述，我们计划继续优化Animated，以使得它更加高性能。我们还想尝试一些声明式的手势响应和触发动画，譬如垂直或者水平的倾斜操作。

上面的API提供了一个强大的工具来简明、粗暴、高效地组织各种各种不同的动画。你可以在[UIExplorer/AnimationExample](https://github.com/facebook/react-native/tree/master/Examples/UIExplorer/AnimatedGratuitousApp)中看到更多的样例代码。不过还有些时候`Animated`并不能支持你想要的效果，下面的章节包含了一些其它的动画系统。

### LayoutAnimation (仅限iOS)

`LayoutAnimation`允许你全局地配置在下一次渲染/布局周期运行的`创建`和`更新`动画，它可以在flexbox布局更新而不影响测量或者计算任何特殊的属性的情况下直接产生动画，尤其是当布局变化可能影响到子节点（譬如“查看更多”展开动画既增加父节点的尺寸又会将位于本行之下的所有行向下推动）时，如果不使用`LayoutAnimation`，可能就需要显式声明组件的坐标，才能使得动画可以同步运行。

注意尽管`LayoutAnimation`非常强大且有用，它会使得你对动画本身的控制没有`Animated`或者其它动画库那样方便，所以如果你使用`LayoutAnimation`无法实现一个效果，你可能还是要使用其他的方案。

![](../img/LayoutAnimationExample.gif)

```javascript
var App = React.createClass({
  componentWillMount() {
    // Animate creation
    LayoutAnimation.spring();
  },

  getInitialState() {
    return { w: 100, h: 100 }
  },

  _onPress() {
    // Animate the update
    LayoutAnimation.spring();
    this.setState({w: this.state.w + 15, h: this.state.h + 15})
  },

  render: function() {
    return (
      <View style={styles.container}>
        <View style={[styles.box, {width: this.state.w, height: this.state.h}]} />
        <TouchableOpacity onPress={this._onPress}>
          <View style={styles.button}>
            <Text style={styles.buttonText}>Press me!</Text>
          </View>
        </TouchableOpacity>
      </View>
    );
  }
});
```
[运行这个例子](https://rnplay.org/apps/uaQrGQ)

上面这个例子使用了一个直接数值来控制大小，不过你也可以自己配置你需要的动画。参见[LayoutAnimation.js](https://github.com/facebook/react-native/blob/master/Libraries/LayoutAnimation/LayoutAnimation.js)。

### requestAnimationFrame

`requestAnimationFrame`是一个对浏览器标准API的兼容，你可能已经熟悉它了。它接受一个函数作为唯一的参数，并且在下一次重绘之前调用此函数。这在创建基于JavaScript的动画库的时候非常有用。通常你不必直接调用这个API——动画库会替你管理好帧的更新。

### react-tween-state(不推荐，用[Animated](#animated)来替代)

[react-tween-state](https://github.com/chenglou/react-tween-state)使用一个极小的库，正如它名字（tween：补间）表示的含义：它生成一个节点的状态的补间值，从一个**开始**值，结束于一个**到达**值。这意味着它生成这两者之间的值，然后在每次`requestAnimationFrame`的时候修改状态。

> 在[Wikipedia](https://en.wikipedia.org/wiki/Inbetweening)上对于补间动画(tweening)的定义：
>
> “补间是在两个图像之间生成中间帧，以使得第一个图像能够平滑的变化为第二个图像”。补间帧是指在关键帧之间用于创建过渡假象的图画。”

一个最基础的从一个值运动到另一个值的办法就是线性过渡：只需要将结束值减去开始值，然后除以动画总共需要经历的帧数，再在每一帧加到当前值上，一直到达到结束值位置。

线性过渡有时候看起来怪异且不自然，所以react-tween-state提供了一系列常用的[过渡函数](http://easings.net/)，你可以用于使你的动画更加自然。

这个库并非跟随React Native一起发布——要在你的工程中使用它，你需要在你的工程目录下执行`npm i react-tween-state --save`来安装它。

```javascript
var tweenState = require('react-tween-state');

var App = React.createClass({
  mixins: [tweenState.Mixin],

  getInitialState() {
    return { opacity: 1 }
  },

  _animateOpacity() {
    this.tweenState('opacity', {
      easing: tweenState.easingTypes.easeOutQuint,
      duration: 1000,
      endValue: this.state.opacity === 0.2 ? 1 : 0.2,
    });
  },

  render() {
    return (
      <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
        <TouchableWithoutFeedback onPress={this._animateOpacity}>
          <View ref={component => this._box = component}
                style={{width: 200, height: 200, backgroundColor: 'red',
                        opacity: this.getTweeningValue('opacity')}} />
        </TouchableWithoutFeedback>
      </View>
    )
  },
});
```
[运行这个例子](https://rnplay.org/apps/4FUQ-A)

![](../img/TweenState.gif)

在上面的例子里我们变化的是透明度，但你可能也猜到了，我们能变化任何数值的值。了解更多react-tween-state的信息，可以参见它的[README](https://github.com/chenglou/react-tween-state)。

#### Rebound (不推荐 - 使用[Animated](#animated)来替代)

[Rebound.js](https://github.com/facebook/rebound-js)是一个[安卓版Rebound](https://github.com/facebook/rebound)的JavaScript移植版。它的方法类似react-tween-state：你有一个起始值，然后定义一个结束值，然后Rebound会生成所有中间的值并用于你的动画。Rebound基于弹性物理模型，你不需要提供一个动画的持续时间，它会自动根据弹性系数、助力、当前值和结束值来计算。Rebound[在React Native内部应用](https://github.com/facebook/react-native/search?utf8=%E2%9C%93&q=rebound)，用于`Navigator`和`WarningBox`。

![](../img/ReboundImage.gif)

需要注意的是Rebound动画可以被终端——如果你在按下动画的过程中释放手指，它会从当前状态弹回初始值。

```javascript
var rebound = require('rebound');

var App = React.createClass({
  // First we initialize the spring and add a listener, which calls
  // setState whenever it updates
  componentWillMount() {
    // Initialize the spring that will drive animations
    this.springSystem = new rebound.SpringSystem();
    this._scrollSpring = this.springSystem.createSpring();
    var springConfig = this._scrollSpring.getSpringConfig();
    springConfig.tension = 230;
    springConfig.friction = 10;

    this._scrollSpring.addListener({
      onSpringUpdate: () => {
        this.setState({scale: this._scrollSpring.getCurrentValue()});
      },
    });

    // Initialize the spring value at 1
    this._scrollSpring.setCurrentValue(1);
  },

  _onPressIn() {
    this._scrollSpring.setEndValue(0.5);
  },

  _onPressOut() {
    this._scrollSpring.setEndValue(1);
  },

  render: function() {
    var imageStyle = {
      width: 250,
      height: 200,
      transform: [{scaleX: this.state.scale}, {scaleY: this.state.scale}],
    };

    var imageUri = "https://facebook.github.io/react-native/img/ReboundExample.png";

    return (
      <View style={styles.container}>
        <TouchableWithoutFeedback onPressIn={this._onPressIn}
                                  onPressOut={this._onPressOut}>
          <Image source={{uri: imageUri}} style={imageStyle} />
        </TouchableWithoutFeedback>
      </View>
    );
  }
});
```

你还可以为弹跳值启用边界，这样它们不会超出，而是会缓缓接近最终值。在上面的例子里，我们可以添加`this._scrollSpring.setOvershootClampingEnabled(true)`来启用边界。参见下面的gif动画来看一个启用了边界的效果：

![](/react-native/img/Rebound.gif) 截图来自
[react-native-scrollable-tab-view](https://github.com/brentvatne/react-native-scrollable-tab-view)。

你可以在[这里](https://rnplay.org/apps/qHU_5w)看到一个类似的例子。

#### 关于setNativeProps

正如[直接访问](direct-manipulation.html)文档所说，`setNativeProps`允许我们直接修改原生组件的属性，而不需要经过`setState`设置状态或者重新渲染整个组件树。

我们可以把这个用在Rebound样例中来更新缩放比例——这在我们要更新的组件有一个非常深的内嵌结构并且没有使用`shouldComponentUpdate`来优化的时候非常有用。

```javascript
// Back inside of the App component, replace the scrollSpring listener
// in componentWillMount with this:
this._scrollSpring.addListener({
  onSpringUpdate: () => {
    if (!this._photo) { return }
    var v = this._scrollSpring.getCurrentValue();
    var newProps = {style: {transform: [{scaleX: v}, {scaleY: v}]}};
    this._photo.setNativeProps(newProps);
  },
});

// Lastly, we update the render function to no longer pass in the
// transform via style (avoid clashes when re-rendering) and to set the
// photo ref
render: function() {
  return (
    <View style={styles.container}>
      <TouchableWithoutFeedback onPressIn={this._onPressIn} onPressOut={this._onPressOut}>
        <Image ref={component => this._photo = component}
               source={{uri: "https://facebook.github.io/react-native/img/ReboundExample.png"}}
               style={{width: 250, height: 200}} />
      </TouchableWithoutFeedback>
    </View>
  );
}
```
[运行这个例子](https://rnplay.org/apps/fUqjAg)

不过你没办法把`setNativeProps`和react-tween-state结合使用，因为更新的补间值自自动被库设置到了状态上——Rebound另一方面则通过`onSprintUpdate`函数每帧给我们提供一个更新后的值。

如果你发现你的动画导致丢帧（低于60帧每秒），你可以尝试使用`setNativeProps`或者`shouldComponentUpdate`来优化它们。你还可能需要将部分计算工作放在动画完成之后进行，这时候使用[InteractionManager](/react-native/docs/interactionmanager.html)。你可以使用应用内开发者菜单中的“FPS Monitor”工具来监控应用的帧率。

### 导航器场景切换

正如文档[导航器对比](navigator-comparison.html#content)所说，`Navigator`使用JavaScript实现，而`NavigatoIOS`则是一个对于`UINavigationController`提供的原生功能的包装。所以这些场景切换动画仅仅对`Navigator`有效。为了在Navigator中重新创建`UINavigationController`所提供的动画并且使之可以被自定义，React Native导出了一个[NavigatorSceneConfigs](https://github.com/facebook/react-native/blob/master/Libraries/CustomComponents/Navigator/NavigatorSceneConfigs.js)API。

```javascript
var SCREEN_WIDTH = require('Dimensions').get('window').width;
var BaseConfig = Navigator.SceneConfigs.FloatFromRight;

var CustomLeftToRightGesture = Object.assign({}, BaseConfig.gestures.pop, {
  // Make it snap back really quickly after canceling pop
  snapVelocity: 8,

  // Make it so we can drag anywhere on the screen
  edgeHitWidth: SCREEN_WIDTH,
});

var CustomSceneConfig = Object.assign({}, BaseConfig, {
  // A very tightly wound spring will make this transition fast
  springTension: 100,
  springFriction: 1,

  // Use our custom gesture defined above
  gestures: {
    pop: CustomLeftToRightGesture,
  }
});
```
[运行这个例子](https://rnplay.org/apps/HPy6UA)

要了解更多有关自定义场景切换的信息，你可以[阅读相应的源码](https://github.com/facebook/react-native/blob/master/Libraries/CustomComponents/Navigator/NavigatorSceneConfigs.js)。