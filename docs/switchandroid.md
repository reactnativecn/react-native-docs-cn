标准安卓的、可以在两个状态中切换的组件。

### 属性

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="view"></a><a href="view.html#props">View props...</a> <a class="hash-link" href="#view">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="disabled"></a>disabled <span class="propType">bool</span> <a class="hash-link" href="#disabled">#</a></h4>
		<div>
			<p>如果为<code>true</code>，这个组件不能进行交互。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onvaluechange"></a>onValueChange <span class="propType">function</span> <a class="hash-link" href="#onvaluechange">#</a></h4>
		<div>
			<p>当值改变的时候调用此回调函数，参数为新的值。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="testid"></a>testID <span class="propType">string</span> <a class="hash-link" href="#testid">#</a></h4>
		<div>
			<p>用来在端到端测试中定位此视图。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="value"></a>value <span class="propType">bool</span> <a class="hash-link" href="#value">#</a></h4>
		<div>
			<p>表示此开关是否打开。</p>
		</div>
	</div>
</div>


### 例子

```javascript
'use strict';

var React = require('React');

var SwitchAndroid = require('SwitchAndroid');
var Text = require('Text');
var UIExplorerBlock = require('UIExplorerBlock');
var UIExplorerPage = require('UIExplorerPage');

var SwitchAndroidExample = React.createClass({
  statics: {
    title: '<SwitchAndroid>',
    description: 'Standard Android two-state toggle component.'
  },

  getInitialState : function() {
    return {
      trueSwitchIsOn: true,
      falseSwitchIsOn: false,
      colorTrueSwitchIsOn: true,
      colorFalseSwitchIsOn: false,
      eventSwitchIsOn: false,
    };
  },

  render: function() {
    return (
      <UIExplorerPage title="<SwitchAndroid>">
        <UIExplorerBlock title="Switches can be set to true or false">
          <SwitchAndroid
            onValueChange={(value) => this.setState({falseSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.falseSwitchIsOn} />
          <SwitchAndroid
            onValueChange={(value) => this.setState({trueSwitchIsOn: value})}
            value={this.state.trueSwitchIsOn} />
        </UIExplorerBlock>
        <UIExplorerBlock title="Switches can be disabled">
          <SwitchAndroid
            disabled={true}
            style={{marginBottom: 10}}
            value={true} />
          <SwitchAndroid
            disabled={true}
            value={false} />
        </UIExplorerBlock>
        <UIExplorerBlock title="Change events can be detected">
          <SwitchAndroid
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
          <SwitchAndroid
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
          <Text>{this.state.eventSwitchIsOn ? 'On' : 'Off'}</Text>
        </UIExplorerBlock>
        <UIExplorerBlock title="Switches are controlled components">
          <SwitchAndroid />
          <SwitchAndroid value={true} />
        </UIExplorerBlock>
      </UIExplorerPage>
    );
  }
});

module.exports = SwitchAndroidExample;
```