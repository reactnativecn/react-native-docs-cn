创建一个原生的WebView，可以用于访问一个网页。

注意WebView目前还仅在iOS平台支持，参见[已知问题](/docs/known-issues.html)

### 属性

<div class="props">
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="automaticallyadjustcontentinsets"></a>automaticallyAdjustContentInsets <span class="propType">bool</span> <a class="hash-link" href="#automaticallyadjustcontentinsets">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="bounces"></a>bounces <span class="propType">bool</span> <a class="hash-link" href="#bounces">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="contentinset"></a>contentInset <span class="propType">{top: number, left: number, bottom: number, right: number}</span> <a class="hash-link" href="#contentinset">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="html"></a>html <span class="propType">string</span> <a class="hash-link" href="#html">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="injectedjavascript"></a>injectedJavaScript <span class="propType">string</span> <a class="hash-link" href="#injectedjavascript">#</a></h4>
		<div>
			<p>设置在网页加载之前注入的一段JS代码。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="javascriptenabledandroid"></a>javaScriptEnabledAndroid <span class="propType">bool</span> <a class="hash-link" href="#javascriptenabledandroid">#</a></h4>
		<div>
			<p>仅限Android平台。iOS平台JavaScript是默认开启的。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onnavigationstatechange"></a>onNavigationStateChange <span class="propType">function</span> <a class="hash-link" href="#onnavigationstatechange">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="onshouldstartloadwithrequest"></a>onShouldStartLoadWithRequest <span class="propType">function</span> <a class="hash-link" href="#onshouldstartloadwithrequest">#</a></h4>
		<div>
			<p>允许为webview发起的请求运行一个自定义的处理函数。返回true或false表示是否要继续执行响应的请求。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="rendererror"></a>renderError <span class="propType">function</span> <a class="hash-link" href="#rendererror">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="renderloading"></a>renderLoading <span class="propType">function</span> <a class="hash-link" href="#renderloading">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="scalespagetofit"></a>scalesPageToFit <span class="propType">bool</span> <a class="hash-link" href="#scalespagetofit">#</a></h4>
		<div>
			<p>仅限iOS平台，设置是否要把网页缩放到适应视图的大小，以及是否允许用户改变缩放比例。</p>
		</div>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="scrollenabled"></a>scrollEnabled <span class="propType">bool</span> <a class="hash-link" href="#scrollenabled">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="startinloadingstate"></a>startInLoadingState <span class="propType">bool</span> <a class="hash-link" href="#startinloadingstate">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="style"></a>style <span class="propType"><a href="view.html#style">View#style</a></span> <a class="hash-link" href="#style">#</a></h4>
	</div>
	<div class="prop">
		<h4 class="propTitle"><a class="anchor" name="url"></a>url <span class="propType">string</span> <a class="hash-link" href="#url">#</a></h4>
	</div>
</div>