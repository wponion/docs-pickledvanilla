# Migration Guide

While domQ doesn't implement every feature that jQuery provides, everything that it implements should be compatible with jQuery, but there are some minor differences to be aware of and they are listed in this document.

## Attributes

### Booleans

Boolean attributes are: `checked`, `selected`, `async`, `autofocus`, `autoplay`, `controls`, `defer`, `disabled`, `hidden`, `ismap`, `loop`, `multiple`, `open`, `readonly`, `required`, `scoped`.

jQuery handles boolean attributes specially, potentially setting a different value than the one you passed it and updating the corresponding properties as well.

domQ just handles them like any other attribute instead.

## CSS

### Relative values

jQuery supports relative CSS values.

```javascript
$('#foo').css ( 'padding-left', '+=10' );
```

domQ doesn't support this.

## Data

### Caching

jQuery's `$.fn.data` function caches retrieved values, and doesn't refresh them when they are updated outside of jQuery \(e.g. via the `dataset` API\), this makes jQuery's `$.fn.data` function unusable with libraries like React.

domQ doesn't implement such caching functionality and doesn't have this problem, the retrieved values are always fresh.

Also values set via domQ's `$.fn.data` function are stored as JSON values in `data-*` attributes set on the DOM nodes, so for instance calling `$('#foo').data ( 'test', 123 )` will add the `data-test="123"` attribute to the `#foo` node, as a consequence of this values that are not JSON-serializable are not supported.

### Plain objects

jQuery supports handling data on plain objects.

```javascript
$({}).data ( 'foo', 123 );
```

domQ doesn't support this.

## Dimensions

### Hidden elements

If you're trying to retrieve the width/height of an hidden element jQuery will briefly try to render it in order to compute it's dimension, this is unreliable and should be avoided.

domQ doesn't implement such functionality.

If you need this anyway you'll have to show/hide the element on your own:

```javascript
// jQuery
$('#foo').width ();
// domQ
$('#foo').show ();
$('#foo').width ();
$('#foo').hide ();
```

### Negative dimension

A negative width/height gets automatically converted to `0` by jQuery, both when setting it via `$.fn.width|height` or `$.fn.css`.

domQ discourages you from setting a negative width/height, if you want this to work like jQuery you'll have to convert negative values to `0` on your own:

```javascript
// jQuery
$('#foo').width ( myWidth );
$('#foo').css ( width, myWidth );
$('#foo').css ({ width: myWidth });
// domQ
myWidth = Math.max ( 0, parseFloat ( myWidth ) );
$('#foo').width ( myWidth );
$('#foo').css ( width, myWidth );
$('#foo').css ({ width: myWidth });
```

### Transformed element

jQuery ignores any `transform` applied to the element when computing its dimensions.

domQ doesn't.

## Events

domQ's event system relies heavily on the browser's underlying event system so there are some differences when comparing it with jQuery's.

### Custom methods

jQuery provides some custom event methods.

```javascript
event.isDefaultPrevented ();
event.isPropagationStopped ();
event.isImmediatePropagationStopped ();
event.originalEvent;
```

domQ doesn't provide them, as it simply passes along the raw event object instead.

```javascript
event.defaultPrevented;
event.cancelBubble;
// No way of knowing if `stopImmediatePropagation` was called
event;
```

### Non-bubbling events

Some native events don't actually bubble at the browser level, for historical reasons: `focus`, `blur`, `mouseenter` and `mouseleave`, but alternative bubbling versions of these events exist: `focusin`, `focusout`, `mouseover` and `mouseout`.

Both jQuery and domQ let you use the non-bubbling version of these events and transparently make them bubble, however there are some minor differences:

* In domQ if you pass `$.fn.on` the non-bubbling version of an event, and then pass `$.fn.trigger` the bubbling version of that event, the event handler registered for the non-bubbling version of that event won't be triggered, you must use event names consistently.
  * In jQuery depending on the circumstances the event handler registered for the non-bubbling version of an event could be triggered even when then triggering the bubbling version of that event.
* In domQ as long as you use non-bubbling events with the provided event functions, like `$.fn.on` and `$.fn.trigger`, they will always be made bubble. If you instead trigger non-bubbling events manually, like by calling the `focus` method of an element, that event won't be made bubble.
  * In jQuery natively-triggered non-bubbling events will be made bubble too.

Stopping propagation from a delegated event handler

In domQ when using event delegation calling `event.stopPropagation` or returning `false` stops the propagation from the target element, not the delegate element.

There's no perfect workaround for this unfortunately, but in most practical cases you could call `event.stopImmediatePropagation` instead.

```javascript
// jQuery
$('#foo').on ( 'click', '.bar', event => false ); // First function called
$('#foo').on ( 'click', '.bar', event => {} ); // Second function called
$('#foo').on ( 'click', event => {} ); // Function never called

$('.bar').trigger ( 'click' );



// domQ
$('#foo').on ( 'click', '.bar', event => false ); // First function called
$('#foo').on ( 'click', '.bar', event => {} ); // Second function called
$('#foo').on ( 'click', event => {} ); // Third function called

$('.bar').trigger ( 'click' );


// domQ + "stopImmediatePropagation"
$('#foo').on ( 'click', '.bar', event => {
  event.stopImmediatePropagation ();
}); // First function called
$('#foo').on ( 'click', '.bar', event => {} ); // Function never called
$('#foo').on ( 'click', event => {} ); // Function never called

$('.bar').trigger ( 'click' );
```

### Trigger data

jQuery supports passing multiple data arguments to your event handlers by providing an array of data arguments to `$.fn.trigger`.

domQ doesn't support this, whatever you provide as a data argument will be passed through as is, even if it's an array.

### Plain objects

jQuery supports handling events on plain objects.

```javascript
$({}).on ( 'foo', () => {} );
```

domQ doesn't support this.

## Manipulation

#### Inserting plain text <a id="inserting-plain-text"></a>

jQuery supports inserting plain text using different methods \(`$.fn.after`, `$.fn.append` etc.\).

```javascript
$('.foo').append ( 'something' );
```

domQ doesn't support that because it instead supports receiving a selector as an argument, and that can be ambiguous when also supporting plain text.

```javascript
$('.foo').append ( '.foo' ); 
// Is that a target or do we actually wanto to append ".foo"?
```

In domQ you should generally wrap your plain texts in a `<span>` element, or create a `textNode` node manually.

```javascript
$('.foo').append ( '<span>something</span>' );
$('.foo').append ( document.createTextNode ( 'something' ) );
```

## Parsing

### Malformed HTML

jQuery smooths over many details and attempts to correctly parse malformed HTML.

```javascript
$('<div/><hr/><code/><b/>').length // => 4
```

domQ on the other hand for the most part just lets the browser handle your HTML directly, so it behaves more strictly and you should be more careful about the HTML strings you're passing to it.

```javascript
$('<div/><hr/><code/><b/>').length // => 1
```

### Selectors <a id="selectors"></a>

#### Custom selectors <a id="custom-selectors"></a>

jQuery implements many custom selectors, like `:hidden`.

domQ only supports selectors the browser recognizes as valid, everything else will throw an error.

#### Binary operators <a id="binary-operators"></a>

Some CSS operators are binary, they operate on something before and after them: `>`, `~`, `+`.

jQuery allows you to use them at the beginning of your selectors inside `$.fn.find`, in a unary fashion.

domQ only supports selectors the browser recognizes as valid, so you can't just use `> .bar` like you sometimes can with jQuery.

If you only target modern browsers you could use the [`:scope`](https://developer.mozilla.org/en-US/docs/Web/CSS/:scope) CSS pseudo-class.

```javascript
// jQuery
$('#foo').find ( '> .bar' );
$('#foo').find ( '~ .bar' );
$('#foo').find ( '+ .bar' );

// domQ
$('#foo').children ( '.bar' );
$('#foo').nextAll ( '.bar' );
$('#foo').next ( '.bar' );

// domQ + ":scope"
$('#foo').find ( ':scope > .bar' );
$('#foo').find ( ':scope ~ .bar' );
$('#foo').find ( ':scope + .bar' );
```

## Utilities

### Unique

jQuery's `$.unique` function only works with DOM nodes.

domQ's `$.unique` function works with any kind of value.

## Others

Other general differences to be aware of.

### Disconnected nodes, iframes and SVGs

jQuery handles specially disconnected nodes, iframes and SVGs, smoothing over many rough corners and just making their APIs "work".

domQ doesn't smooth over as many rough corners \(yet?\), so you should test more carefully the portions of your code that deal with those kind of objects.

### Function that returns a value

jQuery accepts in many methods a function that returns a value, other than just the value itself.

```javascript
$('#foo').attr ( 'bar', () => Math.random () );
```

domQ doesn't support this.

### [Sort order](https://hmble.github.io/cash/#/migration_guide?id=sort-order)

Elements inside domQ and jQuery collections may be sorted differently.

