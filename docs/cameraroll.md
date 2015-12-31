`CameraRoll`模块提供了访问本地相册的功能。

### 截图
![cameraroll](../img/api/cameraroll.png)

### 方法

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="saveimagewithtag"></a><span class="propType">static </span>saveImageWithTag<span class="propType">(tag, successCallback, errorCallback)</span> <a class="hash-link" href="#saveimagewithtag">#</a></h4>
		<div>
			<p>保存一个图片到相册。</p>
			<p>CameraRoll API目前还未实现安卓版本。</p>
			<p>@param {string} tag 在安卓上，本参数是一个本地URI，例如<code>"file:///sdcard/img.png"</code>.</p>
			<p>在iOS设备上可能是以下之一：</p>
			<ul>
				<li>本地URI</li>
				<li>资源库的标签</li>
				<li>非以上两种类型，表示图片数据将会存储在内存中（并且在本进程持续的时候一直会占用内存）。</li>
			</ul>
			<p>@param successCallback 当成功的时候调用，参数为<code>tag</code>。</p>
			<p>@param errorCallback 当失败的时候调用，参数为一个错误信息字符串。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="getphotos"></a><span class="propType">static </span>getPhotos<span class="propType">(params, callback, errorCallback)</span> <a class="hash-link" href="#getphotos">#</a></h4>
		<div>
			<p> 调用<code>callback</code>回调函数，参数是从设备本地相册中读取的图片对象列表。对象的定义参见<a href="https://github.com/facebook/react-native/blob/master/Libraries/CameraRoll/CameraRoll.js#L84" target="_blank"><code>getPhotosReturnChecker</code></a>。</p>
			<p> @param {object} params 参见<a href="https://github.com/facebook/react-native/blob/master/Libraries/CameraRoll/CameraRoll.js#L46" target="_blank"><code>getPhotosParamChecker</code></a>. </p>
			<p> @param {function} callback 当成功的时候调用，参数对象的定义参见<code>getPhotosReturnChecker</code>。</p>
			<p> @param {function} errorCallback 当失败的时候调用，参数为一个错误信息字符串。</p>
		</div>
	</div>
</div>