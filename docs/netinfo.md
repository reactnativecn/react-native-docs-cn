### 方法

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="addeventlistener"></a><span class="propType">static </span>addEventListener<span class="propType">(eventName: ChangeEventName, handler: Function)</span> <a class="hash-link" href="#addeventlistener">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="removeeventlistener"></a><span class="propType">static </span>removeEventListener<span class="propType">(eventName: ChangeEventName, handler: Function)</span> <a class="hash-link" href="#removeeventlistener">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="fetch"></a><span class="propType">static </span>fetch<span class="propType">()</span> <a class="hash-link" href="#fetch">#</a></h4></div>
</div>

### 属性

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="isconnected"></a>isConnected<span class="propType">: ObjectExpression</span> <a class="hash-link" href="#isconnected">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="isconnectionmetered"></a>isConnectionMetered<span class="propType">: TypeCastExpression</span> <a class="hash-link" href="#isconnectionmetered">#</a></h4></div>
</div>

### 例子

```javascript
'use strict';

var React = require('react-native');
var {
  NetInfo,
  Text,
  View
} = React;

var ReachabilitySubscription = React.createClass({
  getInitialState() {
    return {
      reachabilityHistory: [],
    };
  },
  componentDidMount: function() {
    NetInfo.addEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  componentWillUnmount: function() {
    NetInfo.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    var reachabilityHistory = this.state.reachabilityHistory.slice();
    reachabilityHistory.push(reachability);
    this.setState({
      reachabilityHistory,
    });
  },
  render() {
    return (
      <View>
        <Text>{JSON.stringify(this.state.reachabilityHistory)}</Text>
      </View>
    );
  }
});

var ReachabilityCurrent = React.createClass({
  getInitialState() {
    return {
      reachability: null,
    };
  },
  componentDidMount: function() {
    NetInfo.addEventListener(
      'change',
      this._handleReachabilityChange
    );
    NetInfo.fetch().done(
      (reachability) => { this.setState({reachability}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.removeEventListener(
      'change',
      this._handleReachabilityChange
    );
  },
  _handleReachabilityChange: function(reachability) {
    this.setState({
      reachability,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.reachability}</Text>
      </View>
    );
  }
});

var IsConnected = React.createClass({
  getInitialState() {
    return {
      isConnected: null,
    };
  },
  componentDidMount: function() {
    NetInfo.isConnected.addEventListener(
      'change',
      this._handleConnectivityChange
    );
    NetInfo.isConnected.fetch().done(
      (isConnected) => { this.setState({isConnected}); }
    );
  },
  componentWillUnmount: function() {
    NetInfo.isConnected.removeEventListener(
      'change',
      this._handleConnectivityChange
    );
  },
  _handleConnectivityChange: function(isConnected) {
    this.setState({
      isConnected,
    });
  },
  render() {
    return (
      <View>
        <Text>{this.state.isConnected ? 'Online' : 'Offline'}</Text>
      </View>
    );
  }
});

exports.title = 'NetInfo';
exports.description = 'Monitor network status';
exports.examples = [
  {
    title: 'NetInfo.isConnected',
    description: 'Asynchronously load and observe connectivity',
    render(): ReactElement { return <IsConnected />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Asynchronously load and observe iOS reachability',
    render(): ReactElement { return <ReachabilityCurrent />; }
  },
  {
    title: 'NetInfo.reachabilityIOS',
    description: 'Observed updates to iOS reachability',
    render(): ReactElement { return <ReachabilitySubscription />; }
  },
];
```
