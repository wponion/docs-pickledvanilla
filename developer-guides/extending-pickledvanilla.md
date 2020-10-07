# Extending domQ

domQ can be extended, adding custom methods to domQ collections or custom static methods to the domQ object.

If you're writing JavaScript the way to do it is exactly the same as what you'd do for extending jQuery

## Collection Method

This is how you can add an hypothetical `foo` method to domQ collections:

### Function Declaration

{% tabs %}
{% tab title="Vanilla Javascript" %}
```javascript
( function( window ) {
	window.domQ.fn.foo = function() {
		return this;
	};
} )( window );
```
{% endtab %}

{% tab title="ES Module" %}
```javascript
import domQ from "domQ";

domQ.fn.foo = function() {
	return this;
};
```
{% endtab %}
{% endtabs %}

### Function Callback / Usage

```text
domQ('*').foo();
```

## Static Method

This is how you can add an hypothetical `calcNumbers` static method to the Cash object:

### Function Declaration

{% tabs %}
{% tab title="Vanilla Javascript" %}
```javascript
( function( window ) {
	window.domQ.calcNumbers = function( number1, number2 ) {
		return ( number1 + number2 );
	};
} )( window );

```
{% endtab %}

{% tab title="ES Module" %}
```javascript
import domQ from "domQ";

domQ.calcNumbers = function( number1, number2 ) {
	return ( number1 + number2 );
};
```
{% endtab %}
{% endtabs %}

### Function Callback / Usage

```javascript
domQ.calcNumbers(10 + 2); // returns as 12
```

