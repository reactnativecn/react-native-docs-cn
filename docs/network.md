React Native的目标之一是创造一个实验场，让我们可以实验不同的技术和一些疯狂的想法。因为浏览器环境还不够灵活，我们除了自己实现一遍整个技术栈以外别无选择。在这种情况下，我们也并不打算改变什么，我们希望我们尽可能的忠实于浏览器的API。网络API就是一个典型的例子。

## XMLHttpRequest

XMLHttpRequest基于[iOS网络API](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)实现。需要注意与网页环境不同的地方就是安全机制：你可以随意的访问Internet上的任何网站，因为没有[跨域资源共享](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)的限制。

```javascript
var request = new XMLHttpRequest();
request.onreadystatechange = (e) => {
  if (request.readyState !== 4) {
    return;
  }

  if (request.status === 200) {
    console.log('success', request.responseText);
  } else {
    console.warn('error');
  }
};

request.open('GET', 'https://mywebsite.com/endpoint.php');
request.send();
```

要查阅完整的API描述，请参阅[MDN文档](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)。

作为开发者来说，你通常不应该直接使用XMLHttpRequest，因为它的API用起来非常冗长。不过实现兼容浏览器的API使得你可以使用很多npm上已有的第三方库，例如[Parse]( https://parse.com/products/javascript)或者[super-agent](https://github.com/visionmedia/superagent)。

## Fetch

[fetch](https://fetch.spec.whatwg.org/)是一个更好的网络API，它由标准委员会提出，并已经在Chrome中实现。它在React Native中也默认可以使用。

#### 使用方法

```javascript
fetch('https://mywebsite.com/endpoint/')
```

第二个参数是可选的，它可以自定义HTTP请求中的的一些部分。

```javascript
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  })
})
```

#### 异步操作

`fetch`的返回值是一个[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)对象，你可以用两种办法来使用它：


1. 使用`then`和`catch`指定回调函数

```javascript
fetch('https://mywebsite.com/endpoint.php')
  .then((response) => response.text())
  .then((responseText) => {
    console.log(responseText);
  })
  .catch((error) => {
    console.warn(error);
  });
```

2. 使用ES7的`async`/`await`语法来发起一个异步调用：

```javascript
async getUsersFromApi() {
  try {
    let response = await fetch('https://mywebsite.com/endpoint/');
    return response.users;
  } catch(error) {
    throw error;
  }
}
```

- 注意：Promise可能会被拒绝，此时会抛出一个错误。你需要抓住这个错误，否则可能没有任何提示。

## WebSocket

WebSocket是一种基于TCP连接的完整的双端通讯协议。

```javascript
var ws = new WebSocket('ws://host.com/path');

ws.on('open', function() {
  // connection opened
  ws.send('something');
});

ws.on('message', function(e) {
  // a message was received
  console.log(e.data);
});

ws.on('error', function(e) {
  // an error occurred
  console.log(e.message);
});

ws.on('close', function(e) {
  // connection closed
  console.log(e.code, e.reason);
});
```

