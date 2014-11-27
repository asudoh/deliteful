---
layout: default
title: deliteful/Combobox
---

# deliteful/Combobox

`deliteful/Combobox` is a form-aware and store-aware widget leveraging the 
[`deliteful/list/List`](/deliteful/docs/master/list/List.md) widget for
displaying the list of options. 

Characteristics:
* It allows to benefit from the customization mechanism of the list item rendering.
* Provides single and multiple selection modes.
* Provides optional interactive filtering of list of options (single selection mode only). 
* The rendering of the popup is multi-channel responsive: by default, the popup is displayed
on desktop below/above the main element, while on mobile it is displayed in a centered
overlay.


*Example of deliteful/Combobox (single choice mode, on desktop browser):*

![Example of Combobox (single choice mode)](images/Combobox-single.png)

*Example of deliteful/Combobox (multiple choice mode, on mobile browser):*

![Example of Combobox (multiple choice mode)](images/Combobox-multiple.png)


##### Table of Contents
[Element Instantiation ](#instantiation)  
[Using Combobox](#using)  
[Element Styling](#styling)  
[Enterprise Use](#enterprise)


<a name="instantiation"></a>
## Element Instantiation

For details on the instantiation lifecycle, see [`delite/Widget`](/delite/docs/master/Widget.md).

### Declarative Instantiation

```js
require(["delite/register", "deliteful/Store",
  "deliteful/Combobox", "requirejs-domready/domReady!"],
  function (register) {
    register.parse();
  });
```

```html
<html>
  <d-combobox>
    <d-list store="store"></d-list>
  </d-combobox>
  <d-store id="store">
    { "label": "France", ... },
      ...
  </d-store>
</html>
```

<iframe width="100%" height="300" allowfullscreen="allowfullscreen" frameborder="0" 
src="http://jsfiddle.net/ibmjs/d1sj0fkp/embedded/result,js,html">
<a href="http://jsfiddle.net/ibmjs/d1sj0fkp/">checkout the sample on JSFiddle</a></iframe>


### Programmatic Instantiation


```js
require(["delite/register", "dstore/Memory", "dstore/Trackable",
         "deliteful/Combobox", "deliteful/list/List",
         "requirejs-domready/domReady!"],
  function (register, Memory, Trackable, Combobox, List) {
    register.parse();
    // Create the store
    var dataStore = new (Memory.createSubclass(Trackable))({});
    // Add options
    dataStore.add(...);
    ...
    // Create the List
    var list = new List({store: dataStore, ...});
    // Create the Combobox
    var Combobox = new Combobox({list: list, selectionMode: "multiple"});
    Combobox.placeAt(document.body);      
    Combobox.startup();
});
```

<iframe width="100%" height="300" allowfullscreen="allowfullscreen" frameborder="0" 
src="http://jsfiddle.net/ibmjs/s2fzabtb/embedded/result,js,html">
<a href="http://jsfiddle.net/ibmjs/s2fzabtb/">checkout the sample on JSFiddle</a></iframe>


Note that the `list` property is set by default to a newly created instance of
`deliteful/list/List`. Hence, applications can write:

```js
    var combobox = new Combobox();
    // Create the store
    combobox.list.store = ...;
    ...
```

<a name="using"></a>
## Using Combobox

### Selection Mode

The widget provides two selection modes through the `selectionMode` property: 
"single" (only one option can be selected at a time) and "multiple" (one or more
options can be selected).

### Auto Filtering

In single selection mode, if the property `autoFilter` is set to `true` (default is `false`)
the widget allows to type one or more characters which are used for filtering 
the list of shown list items. By default, the filtering is case-insensitive, and an item
is shown if its label contains the entered string.

The default filtering policy can be customized using the `filterMode` and 
`ignoreCase` properties.

The valid values of `filterMode` are:
		 
* `"startsWith"`: the item matches if its label starts with the filter text.
* `"contains"`: the item matches if its label contains the filter text.
* `"is"`: the item matches if its label is the filter text.

The matching is case insensitive by default. Setting `ignoreCase` to `false` turns
it case sensitive.
 
The filtering is performed by the `filter(fitlerTxt)` method, which is called automatically 
while the user types into the editable input element, with `filterTxt` being the currently
entered text. The default implementation of this method uses `dstore/Filter.match()`.
The matching is performed against the `list.labelAttr` attribute of the data store items.
The method can be overridden for implementing other filtering strategies.
		 
### Attribute Mapping

The customization of the mapping of data store item attributes into render item attributes
can be done on the List instance using the mapping API of 
[`deliteful/list/List`](/deliteful/docs/master/list/List.md) inherited from its superclass
`delite/StoreMap`.

See the [`delite/StoreMap`](/delite/docs/master/StoreMap.md) documentation for
more information about the available mapping options.

### Form support

The widget supports the following form-related properties: `name`, `value`, `disabled` and `alt`,
inherited from [`delite/FormWidget`](/delite/docs/master/FormWidget.md) . 
The submitted value is the `value` property of the widget. 
In single selection mode, the value is the label of the selected option. In multiple
selection mode, the value is an array containing the values of the selection options.
 

<a name="styling"></a>
## Element Styling

### Supported themes

This widget provides default styling for the following delite theme:

* bootstrap

### CSS Classes

CSS classes are bound to the structure of the widget declared in its template `deliteful/Combobox/Combobox.html`.
The following table lists the CSS classes that can be used to style the Combobox widget.

|Class name/selector|Applies to|
|----------|----------|
|d-combobox|Combobox widget node.
|d-combobox-input|The inner native `<input>` node on desktop.
|d-combobox-popup-input|The inner native `input` node inside the centered popup displayed on mobile.
|d-combobox-list|The List widget displayed inside the popup.


<a name="enterprise"></a>
## Enterprise Use

### Accessibility

Keyboard and screen reader accessibility will be supported in the next release.

### Globalization

`deliteful/Combobox` provides an internationalizable bundle that contains the following
messages:
		
|Key|Role|
|----------|----------|
|"multiple-choice"|Text written in the combo in multiple selection mode if more than one item is selected.
|"search-placeholder"|Set as placeholder attribute of the input element used for filtering the list of options.

Right to left orientation is supported by setting the `dir` attribute to `rtl` on the
widget. An issue with the arrow decoration of the combo in RTL will be fixed in the next release. 

### Security

This class has no specific security concern.

### Browser Support

This class supports all supported browsers.