本组件可以在iOS和Android上渲染原生的拾取器（Picker）。用例：
```js
<Picker
  selectedValue={this.state.language}
  onValueChange={(lang) => this.setState({language: lang})}>
  <Picker.Item label="Java" value="java" />
  <Picker.Item label="JavaScript" value="js" />
</Picker>
```

### 属性

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="view"></a><a href="view.html#props">View
        props...</a> <a class="hash-link" href="#view">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="onvaluechange"></a>onValueChange <span
            class="propType">function</span> <a class="hash-link" href="#onvaluechange">#</a></h4>
        <div><p>Callback for when an item is selected. This is called with the following parameters:
            - <code>itemValue</code>: the <code>value</code> prop of the item that was selected
            - <code>itemPosition</code>: the index of the selected item in this picker</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="selectedvalue"></a>selectedValue <span
            class="propType">any</span> <a class="hash-link" href="#selectedvalue">#</a></h4>
        <div><p>Value matching value of one of the items. Can be a string or an integer.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="style"></a>style <span class="propType">pickerStyleType</span>
        <a class="hash-link" href="#style">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="testid"></a>testID <span
            class="propType">string</span> <a class="hash-link" href="#testid">#</a></h4>
        <div><p>Used to locate this view in end-to-end tests.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="enabled"></a><span class="platform">android</span>enabled
        <span class="propType">bool</span> <a class="hash-link" href="#enabled">#</a></h4>
        <div><p>If set to false, the picker will be disabled, i.e. the user will not be able to make a
            selection.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="mode"></a><span class="platform">android</span>mode
        <span class="propType">enum('dialog', 'dropdown')</span> <a class="hash-link" href="#mode">#</a></h4>
        <div><p>On Android, specifies how to display the selection items when the user taps on the picker:</p>
            <ul>
                <li>'dialog': Show a modal dialog. This is the default.</li>
                <li>'dropdown': Shows a dropdown anchored to the picker view</li>
            </ul>
        </div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="prompt"></a><span class="platform">android</span>prompt
        <span class="propType">string</span> <a class="hash-link" href="#prompt">#</a></h4>
        <div><p>Prompt string for this picker, used on Android in dialog mode as the title of the dialog.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="itemstyle"></a><span class="platform">ios</span>itemStyle
        <span class="propType">itemStylePropType</span> <a class="hash-link" href="#itemstyle">#</a></h4>
        <div><p>Style to apply to each of the item labels.</p></div>
    </div>
</div>
