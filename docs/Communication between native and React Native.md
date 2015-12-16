# 在原生和React Native间通信
通过[Integrating with Existing Apps guide](http://facebook.github.io/react-native/docs/embedded-app-ios.html)和[Native UI Components guide](https://facebook.github.io/react-native/docs/native-components-ios.html)，我们学习了React Native和原生组件的互相嵌入，现在我们需要进行两个世界的互相通信。有些方法已经在其他的指南中提到了，这篇文章概述了可用的技术。
## 介绍
React Native是从React中得到的灵感，因此基本的信息流是类似的。在React中信息是单向的。我们维持了一个层次化的组件结构，单个组件仅仅依赖它父母和自己的状态。通过属性（properties）我们将信息从上而下的从父母传递到子元素。如果一个祖先组件需要自己子孙的状态，推荐的方法是传递一个回调函数给对应的子元素。

React Native也运用了相同的概念。只要我们在框架内构建我们的应用，我们就可以构造属性和回调函数。但是，当我们混合React Native和原生组件时，我们需要一些特殊的，跨语言的机制来传递信息。

## 属性
属性是最简单的跨组件通信。因此我们需要一个方法从原生组件传递属性到React Native或者从React Native到原生组件。

### 从原生组件传递属性到React Native

我们使用`RCTRootView`将React Natvie视图封装到原生组件中，`RCTRootView`是一个`UIView`保持了React Native应用。同样它也提供了一个原生和应用的接口。

参数`initialProperties `是`NSDictionary`的一个实例。通过`RCTRootView`的初始化你可以将任意属性传递给React Native应用。参数初始化时被内部转化为一个可供JS组件调用的JSON对象。

	NSArray *imageList = @[@"http://foo.com/bar1.png",
                      @"http://foo.com/bar2.png"];

	NSDictionary *props = @{@"images" : imageList};

	RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                          moduleName:@"ImageBrowserApp"
                                         initialProperties:props];
>            
                                         
	'use strict';
	
	var React = require('react-native');
	  var {
	  View,
	  Image
	} = React;
	
	class ImageBrowserApp extends React.Component {
	  renderImage: function(imgURI) {
	    return (
	      <Image source={{uri: imgURI}} />
	    );
	  },
	  render() {
	    return (
	      <View>
	        {this.props.imageList.map(this.renderImage)}
	      </View>
	    );
	  }
	}
	
	React.AppRegistry.registerComponent('ImageBrowserApp', () => ImageBrowserApp);
                                          
`RCTRootView`同样提供了一个可读写的属性`appProperties`。在`appProperties`设置之后，React Native应用将会重新渲染新的属性。只有新提交的属性和之前的属性有区别时更新才被触发。

	NSArray *imageList = @[@"http://foo.com/bar3.png",
                       @"http://foo.com/bar4.png"];
	rootView.appProperties = @{@"images" : imageList}; 
	
你可以随时更新属性，但是更新必须在主线程中进行，

> 注意：目前，最顶层的RN组件的`componentWillReceiveProps`和`componentWillUpdateProps`方法在属性更新后不会触发。但是，你可以通过`componentWillMount`访问新的属性值 

## 从React Native传递属性到原生组件

这篇[文章](https://facebook.github.io/react-native/docs/native-components-ios.html#properties)讨论了暴露原生组件属性的主要问题。简而言之，在你定制的原生组件中通过`RCT_CUSTOM_VIEW_PROPERTY`宏导出属性，就可以直接在React Native中使用就好像它们是普通的React Native组件一样。

## 属性的限制
跨语言属性的主要缺点是不支持自下而上的回调方法。设想你有一个小的RN视图，当一个JS动作触发时你想从原生的父视图中移除它时，你发现没有办法这么做，因为信息需要从下往上进行传递。

虽然我们有跨语言回调（[在这里描述](https://facebook.github.io/react-native/docs/native-modules-ios.html#callbacks),但是这些回调函数并不一定返回的是我们需要的东西。最主要的问题是它们并不当作属性进行传递。因此，这个机制允许我们从JS触发一个原生动作，然后用JS处理那个动作的处理结果。

## 其他的跨语言交互（事件和原生模块）
如上一章所说，使用属性总会有一些限制。有时候属性并不足以满足应用逻辑，因此我们需要更灵活的解决办法。这一章描述了其他的在React Native中可用的通信方法。他们可以用来内部通信（JS和RN的原生层），也可以用作外部通信（RN和你应用中的纯原生部分）

React Native允许使用跨语言的函数调用。你可以在JS中使用原生代码，也可以在原生代码中调用JS。但是，根据我们工作的角度，我们用不同的方法达到了相同的目的。在原生代码中我们使用事件机制来调度JS中的处理函数，而在React Native中我们直接使用原生模块导出的方法。

### 从原生代码调用React Natvie函数（事件）
详细的时间在这篇[文章](http://facebook.github.io/react-native/docs/native-components-ios.html#events)中进行了讨论。注意使用事件无法确保执行的时间，因为每个事件的处理函数都在不同的线程中执行。

事件很强大，它可以不需要引用直接改变React Native组件。但是，当你使用时要注意下面这些陷阱：

* 由于事件可以从各种地方产生，它们可能导致混乱的依赖
* 事件共享相同的命名空间，因此你可能遇到名字冲突。冲突不会被静态的探测到，因此很难排错
* 如果你对同一个React Native组件使用了多个引用，并且你想区分它们的事件，你很可能需要在事件中同时传递一些标识（你可以使用原生视图中的`reactTag`作为标识）

在React Native中嵌入原生组件时通常的做法是用原生组件的RCTViewManager作为视图的代理，向JS发送事件。这样使相关的事件在同一个地点调用。

### 从React Native中调用原生方法（原生模块）

原生模块是JS中也可以使用的Objective-C类。典型的每一个模块的实例都是通过JS桥进行创建。他们可以导出任意的函数和常量给React Native。详细的细节在参见这篇[文章](https://facebook.github.io/react-native/docs/native-modules-ios.html#content)。

事实上原生模块的单实例限制了嵌入。假设我们有一个React Native组件被嵌入了一个原生视图，并且我们希望更新原生的父视图。使用原生模块机制，我们将导出一个仅含有期望参数的方法，同时也是父视图的标识。这个标识将会用来获得父视图的引用以更新父视图。那样的话，我们需要维持模块中标识到原生模块的映射。
虽然这个解决办法很复杂，它仍被用在了管理所有React Native视图的`RCTUIManager`类中，

原生模块同样可以暴露已有的原生库给JS，[Geolocation library](https://github.com/facebook/react-native/tree/master/Libraries/Geolocation)就是一个现成的例子。

>警告：所有原生模块共享同一个命名空间。创建新模块时注意命名冲突。

## 布局计算流

当集成原生模块和React Natvie时，我们同样需要一个协同不同的布局系统的办法。这一章讨论了常见的布局问题，并且提供了解决机制的简单说明。

### 在React Native中嵌入一个原生组件
这个情况在这篇文章中进行了讨论。基本上，由于react视图是`UIView`的子集，大多数类型和尺寸属性将和你期望的一样可以使用。

### 在原生中嵌入一个React Native组件

#### 固定大小的React Native

最简单的情况是一个对于原生已知的，固定大小React Native应用。特别的，当需要嵌入一个全屏的React Native视图的时候。如果我们需要一个小一点根视图，我们可以明确的设置`RCTRootView`的frame。
比如说，创建一个200像素高，宿主视图那样宽的RN app,我们可以这样：

	// SomeViewController.m
	
	- (void)viewDidLoad
	{
	  [...]
	  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
	                                                   moduleName:appName
	                                            initialProperties:props];
	  rootView.frame = CGMakeRect(0, 0, self.view.width, 200);
	  [self.view addSubview:rootView];
	}

当我们创建了一个固定大小的根视图，我们需要在JS中遵守它的边界。换句话说，我们需要确保React Natvie内容能够在固定的大小中放下。最简单的办法是使用flexbox布局。如果你使用绝对定位，并且React组件在根视图边界外可见，React Native组件将会和原生视图重叠，导致某些不期望的行为。比如说，当你点击根视图边界之外的区域`TouchableHighlight`将不会高亮。
通过重新设置frame的属性来动态更新根视图的大小是完全可行的。React Native将会关注内容布局的变化。

#### 弹性的React Native
有时候我们需要渲染一些不知道大小的内容。假设尺寸将会在JS中动态指定。我们有两个解决办法。

* 你可以将React Natvie包裹在`ScrollView`中。这会保证你的内容总是可以访问，并且不会和原生视图重叠
* React Native允许你在JS中指定RN应用的大小，并且将它传递给拥有者`RCTRootView`。然后拥有者将重新布局子视图，保证UI统一。我们通过设置`RCTRootView`的flexibility模式来达到目的。

`RCTRootView`支持4种不同的弹性模式：

	// RCTRootView.h
	
	typedef NS_ENUM(NSInteger, RCTRootViewSizeFlexibility) {
	  RCTRootViewSizeFlexibilityNone = 0,
	  RCTRootViewSizeFlexibilityWidth,
	  RCTRootViewSizeFlexibilityHeight,
	  RCTRootViewSizeFlexibilityWidthAndHeight,
	};

默认值是`RCTRootViewSizeFlexibilityNone`,表示使用固定大小的根视图（仍然可以通过`setFrame`更改）。其他三种模式可以跟踪React Native尺寸的变化。比如说，设置模式为`RCTRootViewSizeFlexibilityHeight`，React Native将会测量内容的高度然后传递回
`RCTRootView`代理。代理可以执行任意的行为，包括设置根视图的frame,使内容可以大小适合。
代理仅仅在内容的尺寸发生变化时才进行调用。

>注意：在JS和原生中都设置弹性尺寸可能导致不确定的行为。比如--不要在设置`RCTRootView`为`RCTRootViewSizeFlexibilityWidth`时同时指定最顶层的RN组件宽度可变（使用Flexbox）

看一个例子。
	// FlexibleSizeExampleView.m
	
	- (instancetype)initWithFrame:(CGRect)frame
	{
	  [...]
	
	  _rootView = [[RCTRootView alloc] initWithBridge:bridge
	  moduleName:@"FlexibilityExampleApp"
	  initialProperties:@{}];
	
	  _rootView.delegate = self;
	  _rootView.sizeFlexibility = RCTRootViewSizeFlexibilityHeight;
	  _rootView.frame = CGRectMake(0, 0, self.frame.size.width, 0);
	}
	
	#pragma mark - RCTRootViewDelegate
	- (void)rootViewDidChangeIntrinsicSize:(RCTRootView *)rootView
	{
	  CGRect newFrame = rootView.frame;
	  newFrame.size = rootView.intrinsicSize;
	
	  rootView.frame = newFrame;
	}

在例子中我们使用一个`FlexibleSizeExampleView`视图来包含根视图。我们创建了根视图，初始化并且设置了代理。代理将会处理尺寸更新。然后，我们设置根视图的弹性尺寸为`RCTRootViewSizeFlexibilityHeight`，意味着`rootViewDidChangeIntrinsicSize:`方法将会在每次React Native内容高度变化时进行调用。最后，我们设置根视图的宽带和位置。注意我们也在那里设置了高度，但是并没有效果，因为我们已经将高度设置为根据RN内容进行变化了。

你可以在这里查看完整的例子[源代码](https://phabricator.fb.com/diffusion/FBOBJC/browse/master/Libraries/FBReactKit/js/react-native-github/Examples/UIExplorer/UIExplorer/NativeExampleViews/FlexibleSizeExampleView.m)

动态改变根视图的弹性模式是可行的。改变根视图的弹性模式将会导致布局的重新计算，并且当内容尺寸已知时会调用`rootViewDidChangeIntrinsicSize`方法。

>注意：React Native布局是通过一个特殊的线程进行计算，而原生UI视图是通过主线程更新。这可能导致临时的原生组件和React Natvie矛盾。这是一个已知问题，我们的团队已经在着手解决不同源的UI同步更新。
>注意：除非根视图成为其他视图的子视图，否则React Native不会进行任何的布局计算。如果你想在已知React Native尺寸的情况下隐藏视图，请将根视图添加为子视图并且在初始化的时候进行隐（使用`UIView's hidden`属性）藏。然后在代理方法中改变它的可见性。




                                          