本组件会打开一个标准的Android时间选择器的对话框。

### 示例
```js
try {
  const {action, hour, minute} = await TimePickerAndroid.open({
    hour: 14,
    minute: 0,
    is24Hour: false, // 会显示为'2 PM'或'下午2点'
  });
  if (action !== DatePickerAndroid.dismissedAction) {
    // 这里开始可以处理用户选好的时分两个参数：hour (0-23), minute (0-59)
  }
} catch ({code, message}) {
  console.warn('Cannot open time picker', message);
}
```

### 方法

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="open"></a><span
            class="propType">static </span>open<span class="propType">(options: Object)</span> <a class="hash-link"
                                                                                                  href="#open">#</a>
    </h4>
        <div><p>Opens the standard Android time picker dialog.</p>
            <p>The available keys for the <code>options</code> object are:
                <em> <code>hour</code> (0-23) - the hour to show, defaults to the current time
                </em> <code>minute</code> (0-59) - the minute to show, defaults to the current time
                * <code>is24Hour</code> (boolean) - If <code>true</code>, the picker uses the 24-hour format. If <code>false</code>,
                the picker shows an AM/PM chooser. If undefined, the default for the current locale
                is used.</p>
            <p>Returns a Promise which will be invoked an object containing <code>action</code>, <code>hour</code>
                (0-23),
                <code>minute</code> (0-59) if the user picked a time. If the user dismissed the dialog, the Promise will
                still be resolved with action being <code>TimePickerAndroid.dismissedAction</code> and all the other
                keys
                being undefined. <strong>Always</strong> check whether the <code>action</code> before reading the
                values.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="timesetaction"></a><span
            class="propType">static </span>timeSetAction<span class="propType">()</span> <a class="hash-link"
                                                                                            href="#timesetaction">#</a>
    </h4>
        <div><p>A time has been selected.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="dismissedaction"></a><span
            class="propType">static </span>dismissedAction<span class="propType">()</span> <a class="hash-link"
                                                                                              href="#dismissedaction">#</a>
    </h4>
        <div><p>The dialog has been dismissed.</p></div>
    </div>
</div>