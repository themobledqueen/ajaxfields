# ajaxfields.js
**Ajaxfields.js** is a simple Javascript plugin that allows you to turn basic DOM elements into editable input fields.
Initialized fields are activated by a mouse click. The entered value is then sent through an ajax-request and upon its completion the displayed value updates accordingly.

## Demo
Demo can be found at https://themobledqueen.github.io/ajaxfields.js/

## Requirements
jQuery

## Usage

Include the script
```html
<script src="/path/to/ajaxfields.js"></script>
```

Prepare an element
```html
<div class="foo">bar</script>
```

Initialize ajaxfields programatically
```js
let foo = new Ajaxfields.field('.foo', {
  //options
});
```

Or via data-attributes
```html
<div class="foo" data-ajaxfield >bar</div>
```

## Options

#### type `String` `"text"`

Defines the type of a field. Ajaxfields supports several:

| Type | Accepted aliases | Description |
| --- | --- | --- |
| Text | "text", "t"| Creates an `<input type="text"/>` element on click. |
| Textarea | "textarea", "ta" | Creates a `<textarea/>` element on click. |
| Password | "password", "p" | Creates an `<input type="password"/>` element on click. Also hides the text-value of a field in un-activated state (assuming no one in their right mind would store, let alone send, passwords in plain-text). |
| Select | "select", "s" | Creates a `<select/>` element on click. Expects available options to be provided in options property. |
| Date | "date", "d" | Creates an `<input type="date"/>` element on click. |
| Datetime | "datetime", "dt" | Creates an <input type="datetime-local"/> element on click. |
| Time | "time" | Creates an `<input type="time"/>` element on click. |
| Toggle | "toggle", "tog" |Creates a custom `<div class="ajaxfield_toggle"/>` element, that toggles the value between "0" and "1" on click. |

#### url `String`

Absolute URL or a relative path. Acts as the "url" parameter for ajax-request.

#### method `String` `"post"`

Acts as the "method" parameter for ajax-request.

#### props `Object`

Additional custom properties to be sent alongside with a changed value.

#### options `Array|Function|Promise`

Specifies a set of available options for a "Select" field. It could be just a predefined array, but you can also set it up to fetch options from a server (making sure the set is accurate and up-to-date each time) by passing a "thenable" Promise, if you're feeling fancy.

#### updateFromResponse `Boolean` `true`

Whether `Ajaxfields` should update the value on display from a json-response upon completion or to set the one a user entered on the client-side. The former option is a more certain way to ensure accurate communication. Even if you're not planing on sanitizing the input (ew), there are still ways the value may be altered between being sent and returned back. That way, after updating the value in your DB, you can just send back the current state as-is (sanitized, trimmed, truncated, whatever) and be sure the value on the client-side reflects it accurately.

#### checkResponseStatus `Boolean` `true`

Whether `Ajaxfields` should check a suppositive "status" of a response. If such "status" returns as `false` the field recognizes the operation as failed and doesn't update the displayed value.

#### responseStatusCheckFunction `Function(response, ajaxfield)`

Defines a custom function for checking a suppositive "status" of a response. The function must return a boolean value and takes the returned response as its first argument and the field instance as the second one.

#### responseValueKey `String` `"val"`

Specifies a name of the "updated value" key in a returned response. Doesn't do anything unless `updateFromResponse` is set to `true`. If the specified key cannot be located the field falls back on updating the value directly from user input.

#### responseStatusKey `String` `"status"`

Specifies a name of the "operation status" key in a returned response. Doesn't do anything unless `checkResponseStatus` is set to `true`. Considers the absence of such key in the returned response equivalent to it being set to false.

#### groupSelector `Selector` `"[data-ajaxfield-group]"`

Defines a selector for a "group" — an ancestor element of a current field that groups multiple fields together and assigns them common properties such a common id of a corresponding row in your DB table (in case the fields themselves correspond to different columns of it for example). If such element exists, `Ajaxfields` will attempt to get its id and send it along with a changed value.

#### group `Selector|jQuery-object|DOM-object`

Sets a "group" element directly.

#### id `String` `"id"`

A shortcut for specifying an arbitrary "id" of a field to be sent alongside with a changed value. Equivalent to specifying it in `props`.

#### idKey `String` `"id"`

Specifies the name of a key for the above-mentioned `id` property.

#### fieldname `String`

A shortcut for specifying an arbitrary "name" of a field to be sent alongside with a changed value. Equivalent to specifying it in `props`.

#### fieldnameKey `String` `"fieldname"`

Specifies the name of a key for the above-mentioned `fieldname` property.

#### valueKey `String` `"val"`

Specifies a name of the "changed value" key in ajax-request.

#### absolutePositioning `Boolean` `true`

Whether an automatically created input element should be positioned "absolutely" within the field container.

#### onInit `Function(ajaxfield)`

Fires when the field is initialized.

#### onReady `Function(ajaxfield)`

Fires when the field returns to a "ready" state.

#### onSuccess `Function(ajaxfield, response, ajaxdata)`

Fires when the field is successfully updated.

#### onError `Function(ajaxfield, response, ajaxdata)`

Fires when the field fails to update.

#### onComplete `Function(ajaxfield, response, ajaxdata)`

Fires straight after `onSuccess` or `onError` independently from the result.

## Methods

#### getInstanceId()

Returns an id of the field instance. Id is assigned automatically.

#### updateValue(value)

Updates the field with a given value.

#### getObject()

Returns an element (jQuery object) associated with the field.

#### getOptions()

Returns a "thenable" `Promise` that resolves with an array of options provided for a given "Select" field.

## Global methods

#### setBaseURL(url)

Sets the global baseURL. If a field has a relative path defined as its url it will be pre-pended with the global baseURL to form a complete url string. The default value is whatever `window.location.origin` returns.

#### setIdKey(key)

Specifies the default `idKey` property globally.

#### setFieldnameKey(key)

Specifies the default `fieldnameKey` property globally.

#### setValueKey(key)

Specifies the default `valueKey` property globally.

#### get(id)

Returns ajaxfield instance with a given id.

## Data-attributes

You can initialize a field by setting `data-ajaxfield` attribute. Field options are provided by supplying further data-attributes with `af-*` prefix like so: 
```html
<div data-ajaxfield data-af-type="s" data-af-fieldname="foo" data-af-options="['bar', 'baz']">bar</div>
```

By having an ancestor element with data-ajaxfield-group attribute you can apply the same set of options to multiple fields: 
```html
<div data-ajaxfield-group data-af-url="some/common/url">
  <div data-ajaxfield data-af-type="s" data-af-fieldname="foo_1" data-af-options="['bar', 'baz']">bar</div>
  <div data-ajaxfield data-af-type="t" data-af-fieldname="foo_2">qux</div>
</div>
```
