使用`SwitchIOS`，你可以显示一个开关按钮。这是一个受约束的(controlled)组件，所以你必须覆盖`onValueChange`回调，并在其中立即更新`value`属性来确保属性更新，否则用户的操作很快就会被还原，来确保与`props.value`属性一致。

### 属性

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="view"></a><a href="view.html#props">View props...</a> <a class="hash-link" href="#view">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="disabled"></a>disabled <span class="propType">bool</span> <a class="hash-link" href="#disabled">#</a></h4>
		<div>
			<p>如果此属性为true，则开关不能被切换。默认为false。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="ontintcolor"></a>onTintColor <span class="propType">string</span> <a class="hash-link" href="#ontintcolor">#</a></h4>
		<div>
			<p>当开关被打开的时候的背景颜色。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onvaluechange"></a>onValueChange <span class="propType">function</span> <a class="hash-link" href="#onvaluechange">#</a></h4>
		<div>
			<p>当用户切换开关的时候调用此函数。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="thumbtintcolor"></a>thumbTintColor <span class="propType">string</span> <a class="hash-link" href="#thumbtintcolor">#</a></h4>
		<div>
			<p>开关上圆形按钮的背景颜色。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="tintcolor"></a>tintColor <span class="propType">string</span> <a class="hash-link" href="#tintcolor">#</a></h4>
		<div>
			<p>当开关关闭的时候的背景颜色。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="value"></a>value <span class="propType">bool</span> <a class="hash-link" href="#value">#</a></h4>
		<div>
			<p>开关当前的值。如果值为true，表示开关打开；值为false，表示开关关闭。默认为false。</p>
		</div>
	</div>
</div>

### 例子

```javascript
'use strict';

var React = require('react-native');
var {
  SwitchIOS,
  Text,
  View
} = React;

var BasicSwitchExample = React.createClass({
  getInitialState() {
    return {
      trueSwitchIsOn: true,
      falseSwitchIsOn: false,
    };
  },
  render() {
    return (
      <View>
        <SwitchIOS
          onValueChange={(value) => this.setState({falseSwitchIsOn: value})}
          style={{marginBottom: 10}}
          value={this.state.falseSwitchIsOn} />
        <SwitchIOS
          onValueChange={(value) => this.setState({trueSwitchIsOn: value})}
          value={this.state.trueSwitchIsOn} />
      </View>
    );
  }
});

var DisabledSwitchExample = React.createClass({
  render() {
    return (
      <View>
        <SwitchIOS
          disabled={true}
          style={{marginBottom: 10}}
          value={true} />
        <SwitchIOS
          disabled={true}
          value={false} />
      </View>
    );
  },
});

var ColorSwitchExample = React.createClass({
  getInitialState() {
    return {
      colorTrueSwitchIsOn: true,
      colorFalseSwitchIsOn: false,
    };
  },
  render() {
    return (
      <View>
        <SwitchIOS
          onValueChange={(value) => this.setState({colorFalseSwitchIsOn: value})}
          onTintColor="#00ff00"
          style={{marginBottom: 10}}
          thumbTintColor="#0000ff"
          tintColor="#ff0000"
          value={this.state.colorFalseSwitchIsOn} />
        <SwitchIOS
          onValueChange={(value) => this.setState({colorTrueSwitchIsOn: value})}
          onTintColor="#00ff00"
          thumbTintColor="#0000ff"
          tintColor="#ff0000"
          value={this.state.colorTrueSwitchIsOn} />
      </View>
    );
  },
});

var EventSwitchExample = React.createClass({
  getInitialState() {
    return {
      eventSwitchIsOn: false,
      eventSwitchRegressionIsOn: true,
    };
  },
  render() {
    return (
      <View style={{ flexDirection: 'row', justifyContent: 'space-around' }}>
        <View>
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
            <Text>{this.state.eventSwitchIsOn ? 'On' : 'Off'}</Text>
        </View>
        <View>
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchRegressionIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchRegressionIsOn} />
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchRegressionIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchRegressionIsOn} />
          <Text>{this.state.eventSwitchRegressionIsOn ? 'On' : 'Off'}</Text>
        </View>
      </View>
    );
  }
});

exports.title = '<SwitchIOS>';
exports.displayName = 'SwitchExample';
exports.description = 'Native boolean input';
exports.examples = [
  {
    title: 'Switches can be set to true or false',
    render(): ReactElement { return <BasicSwitchExample />; }
  },
  {
    title: 'Switches can be disabled',
    render(): ReactElement { return <DisabledSwitchExample />; }
  },
  {
    title: 'Custom colors can be provided',
    render(): ReactElement { return <ColorSwitchExample />; }
  },
  {
    title: 'Change events can be detected',
    render(): ReactElement { return <EventSwitchExample />; }
  },
  {
    title: 'Switches are controlled components',
    render(): ReactElement { return <SwitchIOS />; }
  }
];
```
