## 样式
开发react native的时候，不能使用我们常用的css语言，只能使用javascript来定义样式；所有核心的组件都有`style`，样式的key、value和我们在web中使用的`CSS`是差不多的；名称写法上面有部分差异，使用的是驼峰规则，比如：`backgroundColor`来代替css中的`background-color`。

`style`可以是一个普通的javascript object对象。当然，可以赋值一个样式数组（和web css中规则是类似的；后定义的样式覆盖之前的样式，也可以使用继承）

随着组件复杂度的增加，经常使用`StyleSheet.create` 把几个样式表定义在一起；下面是例子：

```javascript
import React, { Component } from 'react';
import { AppRegistry, StyleSheet, Text, View } from 'react-native';

class LotsOfStyles extends Component {
  render() {
    return (
      <View>
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigblue}>just bigblue</Text>
        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
        <Text style={[styles.red, styles.bigblue]}>red, then bigblue</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  bigblue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

AppRegistry.registerComponent('LotsOfStyles', () => LotsOfStyles);
```

这是个常用的模式来定义样式，使用层级的方式；

还有很多方式去修改文本的样式，跳转到完整列表 [组件 text](http://reactnative.cn/docs/0.30/text.html#content)

![image](http://note.youdao.com/favicon.ico)
学过了style，我们下章来学习下如何控制组件的大小把；

