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
* `s.slice(0,-n)` Chars removed from right
* Combine: `s.slice(2,-1)` transforms `${field}` into `field`
* `s.substring(x,y)` Start at x, until char y. Have to use x=0 for the typical 'first y chars' scenario
* `s.slice(x,y)` Same as substring
* `s[0]` treat the string as an array directly
* `s.length` It's a property? Why isn't this a function?
* `s.trim()`, `s.trimLeft()` and `s.trimRight()`
* `s.toLowerCase()` and `s.toUpperCase()` have locale versions too
* `s.substr(x,y)` Start at x, for y chars - BUT DEPRECATED!!! Too bad, I often prefer this

## String search and regex

* `s.includes()`, `s.endsWith()` and `s.startsWith()`
* `s.indexOf()` and `s.lastIndexOf()` 0-indexed and return -1 when not found
* `s.match()` collection array of found regex matches
* `s.matchAll()` iterator of match results, gets weird "..."
* `regex.test(s)` returns true/false

## JSON - to and from

* `JSON.parse(jsonInAString)`
  * Throws error if the string isn't valid json.
  * Optional replacer function param, can be used to manipulate the value on the way in.
* `JSON.parse(jsonObj, (k, v) => {`
  * Can break out key and value on the way in. Cleaner design.
* `JSON.stringify(jsonObject)`
  * First param is the JSON object, second param is a replacer (optional)
  * replacer can be an array `['key2', 'key1']` which filters the JSON to just those keys
  * replacer can be a function `JSON.stringify(jsonObj, (k, v) => {`
    * return value is value, key remains the same
    * return `undefined` from the fn to not include a value

## Manipulating JSON Content

### flatten one value

From an array of objects, create a flat array from only one key:

```javascript
const data = [
    { "key1": "value1-1", "key2": "value1-2" },
    { "key1": "value2-1", "key2": "value2-2" } ];
const key2Values = data.map(item => item.key2);
// result = ['value1-2','value2-2']
```

### filter

resultArray = someArray.filter(item, [indx], [[array]] { })

### find

### de-dupe

From a simple array, eliminate all the duplicates:

```javascript
const origArray = [1, 5, 10, 1, 2, 5, 3];
const oneEachArray = Array.from(new Set(origArray));
// result = [1, 5, 10, 2, 3]
```

But: This is only for equivalent data. Upper/lower case variants are different entries. For more complex cases use a .sort and then .filter using the index for partner de-dupe.

### Dual Assignment

Create 2 vars at once based on an object's properties. Very convenient.

```javascript
const { formSearchPhrase, formSearchSpeaker } = req.body;
```

* Variable names must exactly match the property names for this syntax.
* I don't like this implicit matching coupled with variable naming. But still, cool.

### Traversing

```someElement?.someAttribute?.somethingWithin``` - A speacial version of the "```.```" chaining operator that prevents throwing errors when an object is undefined and just returns undefined. Eliminates the need for wrapping in if clauses, assuming undefined is an acceptable return value.

### Encoding and decoding

* `encodeUri(s)` and `decodeUri(s)`
* `atob(s)` makes a base64 string, `btoa(s)` returns to ASCII
* `escape(s)` and `unescape(s)` globals are now deprecated.

## Iterators

Clean, no index available

```javascript
for (const elem of elems) { }
```

Clean, when the index is needed within the loop, but you have to use the index.

```javascript
for (const indx in elems) { 
  const elem = elems[indx];
}
```

More modern iterator method, but really it's not any different or more efficient

```javascript
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

## Modules

```javascript
import { fn1, fn2 } from './somejsfile';
import * as myUtils from './myutils';

let x=fn1();
let x=myUtils.fn1();

export { someCoolFn };
```

Stylistic choice:

* Put the export at the bottom, not in-line on the function. Seems to be the norm.
* Don't create a default fn for a module. I don't like how it forces a change in usage syntax.
