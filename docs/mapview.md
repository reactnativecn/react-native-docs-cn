### 属性

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="view"></a><a href="view.html#props">View props...</a> <a class="hash-link" href="#view">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="annotations"></a>annotations <span class="propType">[{latitude: number, longitude: number, animateDrop: bool, title: string, subtitle: string, hasLeftCallout: bool, hasRightCallout: bool, onLeftCalloutPress: function, onRightCalloutPress: function, id: string}]</span> <a class="hash-link" href="#annotations">#</a></h4>
		<div>
			<p>地图上的标注点，可以带有标题及副标题。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="legallabelinsets"></a>legalLabelInsets <span class="propType">{top: number, left: number, bottom: number, right: number}</span> <a class="hash-link" href="#legallabelinsets">#</a></h4>
		<div>
			<p>地图上标签的合法范围。默认在地图底部左侧。参见<code>EdgeInsetsPropType.js</code>了解更多信息。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="maptype"></a>mapType <span class="propType">enum('standard', 'satellite', 'hybrid')</span> <a class="hash-link" href="#maptype">#</a></h4>
		<div>
			<p>要显示的地图类型。</p>
			<ul>
				<li>standard: 标准道路地图（默认）</li>
				<li>satellite: 卫星视图。</li>
				<li>hybrid: 卫星视图并附带道路和感兴趣的点标记。</li>
			</ul>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="maxdelta"></a>maxDelta <span class="propType">number</span> <a class="hash-link" href="#maxdelta">#</a></h4>
		<div>
			<p>可以被显示的最大区域尺寸。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="mindelta"></a>minDelta <span class="propType">number</span> <a class="hash-link" href="#mindelta">#</a></h4>
		<div>
			<p>可以被显示的最小区域尺寸。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onannotationpress"></a>onAnnotationPress <span class="propType">function</span> <a class="hash-link" href="#onannotationpress">#</a></h4>
		<div>
			<p>当用户点击地图上的标注之后会调用此回调函数一次。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onregionchange"></a>onRegionChange <span class="propType">function</span> <a class="hash-link" href="#onregionchange">#</a></h4>
		<div>
			<p>在用户拖拽地图的时候持续调用此回调函数。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onregionchangecomplete"></a>onRegionChangeComplete <span class="propType">function</span> <a class="hash-link" href="#onregionchangecomplete">#</a></h4>
		<div>
			<p>当用户停止拖拽地图之后，调用此回调函数一次。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="pitchenabled"></a>pitchEnabled <span class="propType">bool</span> <a class="hash-link" href="#pitchenabled">#</a></h4>
		<div>
			<p>当此属性设为<code>true</code>并且地图上关联了一个有效的镜头时，镜头的抬起角度会使地图平面倾斜。当此属性设为<code>false</code>，镜头的抬起角度会忽略，地图永远都显示为俯视角度。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="region"></a>region <span class="propType">{latitude: number, longitude: number, latitudeDelta: number, longitudeDelta: number}</span> <a class="hash-link" href="#region">#</a></h4>
		<div>
			<p>地图显示的区域。</p>
			<p>区域使用中心的坐标和要显示的范围来定义。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="rotateenabled"></a>rotateEnabled <span class="propType">bool</span> <a class="hash-link" href="#rotateenabled">#</a></h4>
		<div>
			<p>当此属性设为<code>true</code>并且地图上关联了一个有效的镜头时，镜头的朝向角度会用于基于中心点旋转地图平面。当此属性设置为<code>false</code>时，朝向角度会被忽略，并且地图永远都显示为顶部方向为正北方。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="scrollenabled"></a>scrollEnabled <span class="propType">bool</span> <a class="hash-link" href="#scrollenabled">#</a></h4>
		<div>
			<p>如果此属性设为<code>false</code>，用户不能改变地图所显示的区域。默认值为<code>true</code>。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="showsuserlocation"></a>showsUserLocation <span class="propType">bool</span> <a class="hash-link" href="#showsuserlocation">#</a></h4>
		<div>
			<p>如果此属性为<code>true</code>，应用会请求用户当前的位置并且聚焦到该位置。默认值是<code>false</code>。</p>
			<p><strong>注意</strong>：你需要在Info.plist中增加NSLocationWhenInUseUsageDescription字段。否则它会没有<em>任何提示而直接失败</em>！</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="style"></a>style <span class="propType"><a href="view.html#style">View#style</a></span> <a class="hash-link" href="#style">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="zoomenabled"></a>zoomEnabled <span class="propType">bool</span> <a class="hash-link" href="#zoomenabled">#</a></h4>
		<div>
			<p>如果此属性为<code>false</code>，用户则不能旋转/缩放地图。默认值为<code>true</code>。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="active"></a><span class="platform">android</span>active <span class="propType">bool</span> <a class="hash-link" href="#active">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="showscompass"></a><span class="platform">ios</span>showsCompass <span class="propType">bool</span> <a class="hash-link" href="#showscompass">#</a></h4>
		<div>
			<p>如果此属性为<code>false</code>，地图上不会显示指南针。默认值为<code>true</code>。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="showspointsofinterest"></a><span class="platform">ios</span>showsPointsOfInterest <span class="propType">bool</span> <a class="hash-link" href="#showspointsofinterest">#</a></h4>
		<div>
			<p>如果此属性为<code>false</code>，感兴趣的点不会在地图上显示。默认为<code>true</code>。</p>
		</div>
	</div>
</div>

### 样例

```javascript
'use strict';

var React = require('react-native');
var {
  MapView,
  StyleSheet,
  Text,
  TextInput,
  View,
} = React;

var regionText = {
  latitude: '0',
  longitude: '0',
  latitudeDelta: '0',
  longitudeDelta: '0',
};

var MapRegionInput = React.createClass({

  propTypes: {
    region: React.PropTypes.shape({
      latitude: React.PropTypes.number.isRequired,
      longitude: React.PropTypes.number.isRequired,
      latitudeDelta: React.PropTypes.number.isRequired,
      longitudeDelta: React.PropTypes.number.isRequired,
    }),
    onChange: React.PropTypes.func.isRequired,
  },

  getInitialState: function() {
    return {
      region: {
        latitude: 0,
        longitude: 0,
        latitudeDelta: 0,
        longitudeDelta: 0,
      }
    };
  },

  componentWillReceiveProps: function(nextProps) {
    this.setState({
      region: nextProps.region || this.getInitialState().region
    });
  },

  render: function() {
    var region = this.state.region || this.getInitialState().region;
    return (
      <View>
        <View style={styles.row}>
          <Text>
            {'Latitude'}
          </Text>
          <TextInput
            value={'' + region.latitude}
            style={styles.textInput}
            onChange={this._onChangeLatitude}
            selectTextOnFocus={true}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Longitude'}
          </Text>
          <TextInput
            value={'' + region.longitude}
            style={styles.textInput}
            onChange={this._onChangeLongitude}
            selectTextOnFocus={true}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Latitude delta'}
          </Text>
          <TextInput
            value={'' + region.latitudeDelta}
            style={styles.textInput}
            onChange={this._onChangeLatitudeDelta}
            selectTextOnFocus={true}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Longitude delta'}
          </Text>
          <TextInput
            value={'' + region.longitudeDelta}
            style={styles.textInput}
            onChange={this._onChangeLongitudeDelta}
            selectTextOnFocus={true}
          />
        </View>
        <View style={styles.changeButton}>
          <Text onPress={this._change}>
            {'Change'}
          </Text>
        </View>
      </View>
    );
  },

  _onChangeLatitude: function(e) {
    regionText.latitude = e.nativeEvent.text;
  },

  _onChangeLongitude: function(e) {
    regionText.longitude = e.nativeEvent.text;
  },

  _onChangeLatitudeDelta: function(e) {
    regionText.latitudeDelta = e.nativeEvent.text;
  },

  _onChangeLongitudeDelta: function(e) {
    regionText.longitudeDelta = e.nativeEvent.text;
  },

  _change: function() {
    this.setState({
      latitude: parseFloat(regionText.latitude),
      longitude: parseFloat(regionText.longitude),
      latitudeDelta: parseFloat(regionText.latitudeDelta),
      longitudeDelta: parseFloat(regionText.longitudeDelta),
    });
    this.props.onChange(this.state.region);
  },

});

var MapViewExample = React.createClass({

  getInitialState() {
    return {
      mapRegion: null,
      mapRegionInput: null,
      annotations: null,
      isFirstLoad: true,
    };
  },

  render() {
    return (
      <View>
        <MapView
          style={styles.map}
          onRegionChange={this._onRegionChange}
          onRegionChangeComplete={this._onRegionChangeComplete}
          region={this.state.mapRegion || undefined}
          annotations={this.state.annotations || undefined}
        />
        <MapRegionInput
          onChange={this._onRegionInputChanged}
          region={this.state.mapRegionInput || undefined}
        />
      </View>
    );
  },

  _getAnnotations(region) {
    return [{
      longitude: region.longitude,
      latitude: region.latitude,
      title: 'You Are Here',
    }];
  },

  _onRegionChange(region) {
    this.setState({
      mapRegionInput: region,
    });
  },

  _onRegionChangeComplete(region) {
    if (this.state.isFirstLoad) {
      this.setState({
        mapRegionInput: region,
        annotations: this._getAnnotations(region),
        isFirstLoad: false,
      });
    }
  },

  _onRegionInputChanged(region) {
    this.setState({
      mapRegion: region,
      mapRegionInput: region,
      annotations: this._getAnnotations(region),
    });
  },

});

var styles = StyleSheet.create({
  map: {
    height: 150,
    margin: 10,
    borderWidth: 1,
    borderColor: '#000000',
  },
  row: {
    flexDirection: 'row',
    justifyContent: 'space-between',
  },
  textInput: {
    width: 150,
    height: 20,
    borderWidth: 0.5,
    borderColor: '#aaaaaa',
    fontSize: 13,
    padding: 4,
  },
  changeButton: {
    alignSelf: 'center',
    marginTop: 5,
    padding: 3,
    borderWidth: 0.5,
    borderColor: '#777777',
  },
});

exports.displayName = (undefined: ?string);
exports.title = '<MapView>';
exports.description = 'Base component to display maps';
exports.examples = [
  {
    title: 'Map',
    render(): ReactElement { return <MapViewExample />; }
  },
  {
    title: 'Map shows user location',
    render() {
      return  <MapView style={styles.map} showsUserLocation={true} />;
    }
  }
];
```