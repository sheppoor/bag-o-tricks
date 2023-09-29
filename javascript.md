# Javascript

Moved on to modern ECMAScript, leaving all the old workarounds behind (all removed).

## Core definitive sources

* Root and globals <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects>
  * See Control abstraction objects section
* strings <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String>
* regex <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp>
* array <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array>

## String manipulators

* `s.slice(n)` Chars removed from left, so 0-indexed.
* `s.slice(-n)` Chars removed from left, based on the .length. Equivalent to s.slice(s.length-n)
* `s.substr(x,y)` Start at x, for y chars - BUT DEPRECATED!!! Too bad, I often prefer this
* `s.substring(x,y)` Start at x, until char y. Have to use x=0 for the typical 'first y chars' scenario
* `s.slice(x,y)` Same as substring
* `s.trim()`, `s.trimLeft()` and `s.trimRight()`
* `s.toLowerCase()` and `s.toUpperCase()` have locale versions too

## JSON

* `JSON.parse()` takes a string of valid JSON. Throws error if invalid.
  * Optional replacer function param, can be used to manipulate the value on the way in.
  * `JSON.parse(jsonObj, (k, v) => {`
* `JSON.stringify()`
  * First param is the JSON object, second param is a replacer (optional)
  * replacer can be an array `['key2', 'key1']` which filters the JSON to just those keys
  * replacer can be a function `JSON.stringify(jsonObj, (k, v) => {`
    * return value is value, key remains the same
    * return `undefined` from the fn to not include a value

## Working with JSON Content

From an array of objects, create a flat array from only one key:

```javascript
const data = [
    { "key1": "value1-1", "key2": "value1-2" },
    { "key1": "value2-1", "key2": "value2-2" } ];
const key2Values = data.map(item => item.key2);
```

From a simple array, eliminate all the duplicates:

```javascript
const origArray = [1, 5, 10, 1, 2, 5, 3];
const oneEachArray = Array.from(new Set(origArray));
```

Create 2 vars at once based on an object's properties. Very convenient.

* Variable names must exactly match the property names for this syntax.
* I don't like this implicit matching coupled with variable naming. But still cool.

```javascript
const { formSearchPhrase, formSearchSpeaker } = req.body;
```

## String search and regex

* `s.contains()`, `s.endsWith()` and `s.startsWith()`
* `s.indexOf()` and `s.lastIndexOf()` 0-indexed and return -1 when not found
* `s.match()` collection array of found regex matches
* `s.matchAll()` iterator of match results, gets weird "..."

## Encoding and decoding

* `encodeUri(s)` and `decodeUri(s)`
* `atob(s)` makes a base64 string, `btoa(s)` returns to ASCII
* `escape(s)` and `unescape(s)` globals are now deprecated.

## Iterators

My go-to. Kinda old school now. Cleaner. No index available

```javascript
for (const elem of elems) { }
```

Similar, when the index is needed within the loop

```javascript
for (const indx in elems) { 
  const elem = elems[indx];
}
```
More modern iterator method, but really it's not any different or more efficient

```javascreipt
elems.forEach( function(elem) { });
```

Cool version to iterate through an object attributes, breaks out key/value

```javascript
for (const [k, v] of Object.entries(someJsonObj)) { }
```

## Sorting

## Other random snipits favorites

* Optional parameters with a default value: `function x(param=1) { ...`
* `const elems = document.querySelectorAll('.someclass')`
* `const elems = document.querySelectorAll('[data-custom-attribute="123"]');`

`TODO` so many to add...*

<!--
Find all attributes for a js obs
`TODO` THIS IS MY VERY OLD ONE, replace. Also, lots to add.

```Javascript
myobj.each(obj.attributes, function(i, attr){
    try {
        var n = attr.name;
        var v = attr.value;
        console.log (n+": "+v);
    }
    catch(err) {}
    });
```
-->
