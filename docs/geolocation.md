你需要在Info.plist中增加`NSLocationWhenInUseUsageDescription`字段来启用定位功能。如果你使用`react-native init`创建项目，定位会被默认启用。

定位API遵循[MDN规范](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)，你可以在这里查看更详细的说明和例子。

## 方法

#### static getCurrentPosition(geo_success: Function, geo_error?: Function, geo_options?: GeoOptions)

成功时会调用geo_success回调，参数中包含最新的位置信息。

支持的选项：

* timeout (毫秒)
* maximumAge (毫秒)
* enableHighAccurary(boolean)

#### static watchPosition(success: Function, error?: Function, options?: GeoOptions) 

持续监听位置，每当位置变化之后都调用success回调。

支持的选项：

* timeout (毫秒)
* maximumAge (毫秒)
* enableHighAccurary(boolean)

#### static clearWatch(watchID: number)

#### static stopObserving()

## 样例

```javascript
/* eslint no-console: 0 */
'use strict';


var React = require('react-native');
var {
  StyleSheet,
  Text,
  View,
} = React;

exports.framework = 'React';
exports.title = 'Geolocation';
exports.description = 'Examples of using the Geolocation API.';

exports.examples = [
  {
    title: 'navigator.geolocation',
    render: function(): ReactElement {
      return <GeolocationExample />;
    },
  }
];

var GeolocationExample = React.createClass({
  watchID: (null: ?number),

  getInitialState: function() {
    return {
      initialPosition: 'unknown',
      lastPosition: 'unknown',
    };
  },

  componentDidMount: function() {
    navigator.geolocation.getCurrentPosition(
      (initialPosition) => this.setState({initialPosition}),
      (error) => alert(error.message),
      {enableHighAccuracy: true, timeout: 20000, maximumAge: 1000}
    );
    this.watchID = navigator.geolocation.watchPosition((lastPosition) => {
      this.setState({lastPosition});
    });
  },

  componentWillUnmount: function() {
    navigator.geolocation.clearWatch(this.watchID);
  },

  render: function() {
    return (
      <View>
        <Text>
          <Text style={styles.title}>Initial position: </Text>
          {JSON.stringify(this.state.initialPosition)}
        </Text>
        <Text>
          <Text style={styles.title}>Current position: </Text>
          {JSON.stringify(this.state.lastPosition)}
        </Text>
      </View>
    );
  }
});

var styles = StyleSheet.create({
  title: {
    fontWeight: '500',
  },
});
```

