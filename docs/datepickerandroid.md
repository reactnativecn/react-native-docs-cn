本组件会打开一个标准的Android日期选择器的对话框。

### 示例
```js
try {
  const {action, year, month, day} = await DatePickerAndroid.open({
    // 要设置默认值为今天的话，使用`new Date()`即可。
    // 下面显示的会是2020年5月25日。月份是从0开始算的。
    date: new Date(2020, 4, 25)
  });
  if (action !== DatePickerAndroid.dismissedAction) {
    // 这里开始可以处理用户选好的年月日三个参数：year, month (0-11), day
  }
} catch ({code, message}) {
  console.warn('Cannot open date picker', message);
}
```

### 方法

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="open"></a><span
            class="propType">static </span>open<span class="propType">(options: Object)</span> <a class="hash-link"
                                                                                                  href="#open">#</a>
    </h4>
        <div><p>Opens the standard Android date picker dialog.</p>
            <p>The available keys for the <code>options</code> object are:
                <em> <code>date</code> (<code>Date</code> object or timestamp in milliseconds) - date to show by default
                </em> <code>minDate</code> (<code>Date</code> or timestamp in milliseconds) - minimum date that can be
                selected
                * <code>maxDate</code> (<code>Date</code> object or timestamp in milliseconds) - minimum date that can
                be selected</p>
            <p>Returns a Promise which will be invoked an object containing <code>action</code>, <code>year</code>,
                <code>month</code> (0-11),
                <code>day</code> if the user picked a date. If the user dismissed the dialog, the Promise will
                still be resolved with action being <code>DatePickerAndroid.dismissedAction</code> and all the other
                keys
                being undefined. <strong>Always</strong> check whether the <code>action</code> before reading the
                values.</p>
            <p>Note the native date picker dialog has some UI glitches on Android 4 and lower
                when using the <code>minDate</code> and <code>maxDate</code> options.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="datesetaction"></a><span
            class="propType">static </span>dateSetAction<span class="propType">()</span> <a class="hash-link"
                                                                                            href="#datesetaction">#</a>
    </h4>
        <div><p>A date has been selected.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="dismissedaction"></a><span
            class="propType">static </span>dismissedAction<span class="propType">()</span> <a class="hash-link"
                                                                                              href="#dismissedaction">#</a>
    </h4>
        <div><p>The dialog has been dismissed.</p></div>
    </div>
</div>