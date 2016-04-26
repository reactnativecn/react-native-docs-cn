Provides efficient data processing and access to the `ListView` component. A `ListViewDataSource` is created with functions for extracting data from the input blob, and comparing elements (with default implementations for convenience). The input blob can be as simple as an array of strings, or an object with rows nested inside section objects.

To update the data in the datasource, use `cloneWithRows` (or `cloneWithRowsAndSections` if you care about sections). The data in the data source is immutable, so you can't modify it directly. The clone methods suck in the new data and compute a diff for each row so ListView knows whether to re-render it or not.

In this example, a component receives data in chunks, handled by `_onDataArrived`, which concats the new data onto the old data and updates the data source. We use concat to create a new array - mutating `this._data`, e.g. with `this._data.push(newRowData)`, would be an error. `_rowHasChanged` understands the shape of the row data and knows how to efficiently compare it.

```javascript
constructor(props) {
  super(props);
  var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
  this.state = {
    ds,
  };
}

_onDataArrived = (newData) => {
  this._data = this._data.concat(newData);
  this.setState({
    ds: this.state.ds.cloneWithRows(this._data)
  });
};
```


### 方法

<div class="props">
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="constructor"></a>constructor<span class="propType">(params)</span>
        <a class="hash-link" href="docs/listviewdatasource.html#constructor">#</a></h4>
        <div><p>You can provide custom extraction and <code>hasChanged</code> functions for section
            headers and rows. If absent, data will be extracted with the
            <code>defaultGetRowData</code> and <code>defaultGetSectionHeaderData</code> functions.</p>
            <p>The default extractor expects data of one of the following forms:</p>
            <div class="prism language-javascript"><span class="token punctuation">{</span> sectionID_1<span
                    class="token punctuation">:</span> <span class="token punctuation">{</span> rowID_1<span
                    class="token punctuation">:</span> &lt;rowData1<span class="token operator">&gt;</span><span
                    class="token punctuation">,</span> <span class="token punctuation">.</span><span
                    class="token punctuation">.</span><span class="token punctuation">.</span> <span
                    class="token punctuation">}</span><span class="token punctuation">,</span> <span
                    class="token punctuation">.</span><span class="token punctuation">.</span><span
                    class="token punctuation">.</span> <span class="token punctuation">}</span></div>
            <p> or</p>
            <div class="prism language-javascript"><span class="token punctuation">{</span> sectionID_1<span
                    class="token punctuation">:</span> <span class="token punctuation">[</span> &lt;rowData1<span
                    class="token operator">&gt;</span><span class="token punctuation">,</span> &lt;rowData2<span
                    class="token operator">&gt;</span><span class="token punctuation">,</span> <span
                    class="token punctuation">.</span><span class="token punctuation">.</span><span
                    class="token punctuation">.</span> <span class="token punctuation">]</span><span
                    class="token punctuation">,</span> <span class="token punctuation">.</span><span
                    class="token punctuation">.</span><span class="token punctuation">.</span> <span
                    class="token punctuation">}</span></div>
            <p> or</p>
            <div class="prism language-javascript"><span class="token punctuation">[</span> <span
                    class="token punctuation">[</span> &lt;rowData1<span class="token operator">&gt;</span><span
                    class="token punctuation">,</span> &lt;rowData2<span class="token operator">&gt;</span><span
                    class="token punctuation">,</span> <span class="token punctuation">.</span><span
                    class="token punctuation">.</span><span class="token punctuation">.</span> <span
                    class="token punctuation">]</span><span class="token punctuation">,</span> <span
                    class="token punctuation">.</span><span class="token punctuation">.</span><span
                    class="token punctuation">.</span> <span class="token punctuation">]</span></div>
            <p>The constructor takes in a params argument that can contain any of the
                following:</p>
            <ul>
                <li>getRowData(dataBlob, sectionID, rowID);</li>
                <li>getSectionHeaderData(dataBlob, sectionID);</li>
                <li>rowHasChanged(prevRowData, nextRowData);</li>
                <li>sectionHeaderHasChanged(prevSectionData, nextSectionData);</li>
            </ul>
        </div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="clonewithrows"></a>cloneWithRows<span
            class="propType">(dataBlob, rowIdentities)</span> <a class="hash-link"
                                                                 href="docs/listviewdatasource.html#clonewithrows">#</a>
    </h4>
        <div><p>Clones this <code>ListViewDataSource</code> with the specified <code>dataBlob</code> and
            <code>rowIdentities</code>. The <code>dataBlob</code> is just an arbitrary blob of data. At
            construction an extractor to get the interesting information was defined
            (or the default was used).</p>
            <p>The <code>rowIdentities</code> is is a 2D array of identifiers for rows.
                ie. [['a1', 'a2'], ['b1', 'b2', 'b3'], ...]. If not provided, it's
                assumed that the keys of the section data are the row identities.</p>
            <p>Note: This function does NOT clone the data in this data source. It simply
                passes the functions defined at construction to a new data source with
                the data specified. If you wish to maintain the existing data you must
                handle merging of old and new data separately and then pass that into
                this function as the <code>dataBlob</code>.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="clonewithrowsandsections"></a>cloneWithRowsAndSections<span
            class="propType">(dataBlob, sectionIdentities, rowIdentities)</span> <a class="hash-link"
                                                                                    href="docs/listviewdatasource.html#clonewithrowsandsections">#</a>
    </h4>
        <div><p>This performs the same function as the <code>cloneWithRows</code> function but here
            you also specify what your <code>sectionIdentities</code> are. If you don't care
            about sections you should safely be able to use <code>cloneWithRows</code>.</p>
            <p><code>sectionIdentities</code> is an array of identifiers for sections.
                ie. ['s1', 's2', ...]. If not provided, it's assumed that the
                keys of dataBlob are the section identities.</p>
            <p>Note: this returns a new object!</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getrowcount"></a>getRowCount<span class="propType">()</span>
        <a class="hash-link" href="docs/listviewdatasource.html#getrowcount">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor"
                                               name="getrowandsectioncount"></a>getRowAndSectionCount<span
            class="propType">()</span> <a class="hash-link"
                                          href="docs/listviewdatasource.html#getrowandsectioncount">#</a></h4></div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="rowshouldupdate"></a>rowShouldUpdate<span
            class="propType">(sectionIndex, rowIndex)</span> <a class="hash-link"
                                                                href="docs/listviewdatasource.html#rowshouldupdate">#</a>
    </h4>
        <div><p>Returns if the row is dirtied and needs to be rerendered</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getrowdata"></a>getRowData<span class="propType">(sectionIndex, rowIndex)</span>
        <a class="hash-link" href="docs/listviewdatasource.html#getrowdata">#</a></h4>
        <div><p>Gets the data required to render the row.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getrowidforflatindex"></a>getRowIDForFlatIndex<span
            class="propType">(index)</span> <a class="hash-link"
                                               href="docs/listviewdatasource.html#getrowidforflatindex">#</a></h4>
        <div><p>Gets the rowID at index provided if the dataSource arrays were flattened,
            or null of out of range indexes.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getsectionidforflatindex"></a>getSectionIDForFlatIndex<span
            class="propType">(index)</span> <a class="hash-link"
                                               href="docs/listviewdatasource.html#getsectionidforflatindex">#</a></h4>
        <div><p>Gets the sectionID at index provided if the dataSource arrays were flattened,
            or null for out of range indexes.</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getsectionlengths"></a>getSectionLengths<span
            class="propType">()</span> <a class="hash-link" href="docs/listviewdatasource.html#getsectionlengths">#</a>
    </h4>
        <div><p>Returns an array containing the number of rows in each section</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="sectionheadershouldupdate"></a>sectionHeaderShouldUpdate<span
            class="propType">(sectionIndex)</span> <a class="hash-link"
                                                      href="docs/listviewdatasource.html#sectionheadershouldupdate">#</a>
    </h4>
        <div><p>Returns if the section header is dirtied and needs to be rerendered</p></div>
    </div>
    <div class="prop"><h4 class="propTitle"><a class="anchor" name="getsectionheaderdata"></a>getSectionHeaderData<span
            class="propType">(sectionIndex)</span> <a class="hash-link"
                                                      href="docs/listviewdatasource.html#getsectionheaderdata">#</a>
    </h4>
        <div><p>Gets the data required to render the section header</p></div>
    </div>
</div>
