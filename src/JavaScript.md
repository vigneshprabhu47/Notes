##### 1. List of invisible & zero-length characters (JavaScript character code + unicode string)
```JS
var invChr = [
  [160,   '"\\u00A0"'], // Non-breaking whitespace (HTML entity -> &nbsp;)
  [173,   '"\\u00AD"'],
  [4448,  '"\\u1160"'],
  [8192,  '"\\u2000"'],
  [8193,  '"\\u2001"'],
  [8194,  '"\\u2002"'],
  [8195,  '"\\u2003"'],
  [8196,  '"\\u2004"'],
  [8197,  '"\\u2005"'],
  [8198,  '"\\u2006"'],
  [8199,  '"\\u2007"'],
  [8200,  '"\\u2008"'],
  [8201,  '"\\u2009"'],
  [8202,  '"\\u200A"'],
  [8203,  '"\\u200B"'],
  [8204,  '"\\u200C"'],
  [8205,  '"\\u200D"'],
  [8206,  '"\\u200E"'],
  [8207,  '"\\u200F"'],
  [8234,  '"\\u202A"'],
  [8235,  '"\\u202B"'],
  [8236,  '"\\u202C"'],
  [8237,  '"\\u202D"'],
  [8238,  '"\\u202E"'],
  [8239,  '"\\u202F"'],
  [8287,  '"\\u205F"'],
  [8288,  '"\\u2060"'],
  [8289,  '"\\u2061"'],
  [8290,  '"\\u2062"'],
  [8291,  '"\\u2063"'],
  [8292,  '"\\u2064"'],
  [8293,  '"\\u2065"'],
  [8294,  '"\\u2066"'],
  [8295,  '"\\u2067"'],
  [8296,  '"\\u2068"'],
  [8297,  '"\\u2069"'],
  [8298,  '"\\u206A"'],
  [8299,  '"\\u206B"'],
  [8300,  '"\\u206C"'],
  [8301,  '"\\u206D"'],
  [8302,  '"\\u206E"'],
  [8303,  '"\\u206F"'],
  [12644, '"\\u3164"'],
  [65279, '"\\uFEFF"']
];

for (let x of invChr) {
  console.log(
    x[1],
    x[0],
    `'${JSON.parse(x[1])}'`,
    'Test String'
  );
}
```

------

##### 2. Simple date formats
```JS
const opt1 = { day: "numeric", month: "short", year: "numeric" };
const opt2 = { day: "2-digit", month: "short", year: "numeric" };

console.log(
  new Date("2022-sep-01").toLocaleString("en-IN", opt1) + "\n" +
  // Returns "1 Sep 2022"

  new Date("2022-sep-01").toLocaleString("en-IN", opt2) + "\n" +
  // Returns "01-Sep-2022"

  new Date("2022-sep-01").toLocaleString("en-US", opt1) + "\n" +
  // Returns "Sep 1, 2022"

  new Date("2022-sep-01").toLocaleString("en-US", opt2) + "\n"
  // Returns "Sep 01, 2022"
);
```

------

##### 3. Simple conversions
```JS
var a = [
  undefined,
  null,
  true,
  false,
  "",
  "  ",
  "vipr",
  0,
  60,
  1.6180339887,
  NaN,
  [],
  [1, 2, 3],
  ["A", "D", "M"],
  new Date(),
  {},
  Object.create(null),
  {"a": "m"},
  new Set(),
  new Map(),
  new Map([ ['a', 1], ['d', 3] ]),
  Symbol(),
  Symbol('a'),
  ,  // Increases Array.length by 1, but does not project during map (Empty location).
].map(x => {
  let o = Object.create(null);
  o.Value = x;

  // Unary +
  try {
    o["Numeric (Unary +)"] = +x;
  } catch {
    o["Numeric (Unary +)"] = "ERROR";
  }

  // JSON.stringify
  try {
    o["JSON.stringify"] = JSON.stringify(x);
  } catch {
    o["JSON.stringify"] = "ERROR";
  }

  // Template string for implicit conversion
  try {
    o["String Templating (``)"] = `${x}`;
  } catch {
    o["String Templating (``)"] = "ERROR";
  }

  // Datatype
  try {
    o["Object.prototype.toString.call"] = Object.prototype.toString.call(x);
  } catch {
    o["Object.prototype.toString.call"] = "ERROR";
  }

  return o;
});

console.table(a);
```

Output: `Array(24)`
Expression            | Value          | Numeric (Unary +) | JSON.stringify    | String Templating (\`\`)   | Object.prototype.toString.call
:-----                | :-----         | :-----            | :-----            | :-----              | :-----
`undefined`           | `undefined`    | `NaN`             | `undefined`       | `'undefined'`       |   `'[object Undefined]'`
`null`                | `null`         | `0`               | `'null'`          | `'null'`            |   `'[object Null]'`
`true`                | `true`         | `1`               | `'true'`          | `'true'`            |   `'[object Boolean]'`
`false`               | `false`        | `0`               | `'false'`         | `'false'`           |   `'[object Boolean]'`
`""`                  | `''`           | `0`               | `'""'`            | `''`                |   `'[object String]'`
`"  "`                | `' '`          | `0`               | `'" "'`           | `' '`               |   `'[object String]'`
`"vipr"`              | `'vipr'`       | `NaN`             | `'"vipr"'`        | `'vipr'`            |   `'[object String]'`
`0`                   | `0`            | `0`               | `'0'`             | `'0'`               |   `'[object Number]'`
`60`                  | `60`           | `60`              | `'60'`            | `'60'`              |   `'[object Number]'`
`1.6180339887`        | `1.6180339887` | `1.6180339887`    | `'1.6180339887'`  | `'1.6180339887'`    |   `'[object Number]'`
`NaN`                 | `NaN`          | `NaN`             | `'null'`          | `'NaN'`             |   `'[object Number]'`
`[]`                  | `Array(0)`     | `0`               | `'[]'`            | `''`                |   `'[object Array]'`
`[1, 2, 3]`           | `Array(3)`     | `NaN`             | `'[1,2,3]'`       | `'1,2,3'`           |   `'[object Array]'`
`["A", "D", "M"]`     | `Array(3)`     | `NaN`             | `'["A","D","M"]'` | `'A,D,M'`           |   `'[object Array]'`
`new Date()`          | `Sun Sep 18 2022 18:00:00 GMT+0530 (India Standard Time)` | `1663504200000` | `'"2022-09-18T12:30:00.000Z"'` | `'Sun Sep 18 2022 18:00:00 GMT+0530 (India Standard Time)'` | `'[object Date]'`
`{}`                  | `{…}`          | `NaN`             | `'{}'`            | `'[object Object]'` |   `'[object Object]'`
`Object.create(null)` | `{…}`          | ERROR             | `'{}'`            | ERROR               |   `'[object Object]'`
`{"a": "m"}`          | `{…}`          | `NaN`             | `'{"a":"m"}'`     | `'[object Object]'` |   `'[object Object]'`
`new Set()`           | `Set(0)`       | `NaN`             | `'{}'`            | `'[object Set]'`    |   `'[object Set]'`
`new Map()`           | `Map(0)`       | `NaN`             | `'{}'`            | `'[object Map]'`    |   `'[object Map]'`
`new Map([ ['a', 1], ['d', 3] ])` | `Map(2)` | `NaN` | `'{}'` | `'[object Map]'` | `'[object Map]'`
`Symbol()`            | `Symbol()`     | ERROR             | `undefined`       | ERROR               |   `'[object Symbol]'`
`Symbol('a')`         | `Symbol(a)`    | ERROR             | `undefined`       | ERROR               |   `'[object Symbol]'`

------

##### 4. Code to generate simple, reusable CSS classes
```JS
// START: Helpers
/**
 * Generates a range of integers as per the parameters.\
 * The lower (`start`) and upper (`stop`) bounds are
 * inclusive.\
 * `step` is defaulted to 1.
 * This cannot be zero (use `Array(length).fill(value)`
 * instead).
 */
const range = (start, stop, step = 1) =>
  Array
    .from(
      { length: ((stop - start) / step) + 1 },
      (_, i) => start + (i * step)
    );
// END  : Helpers

// Minify the result or prettify:
var minify = false;

// Update the below objects as needed.
// (class prefix, scale & value range)

// ScalePrefix-Unit map:
const omapScaleUnit = {
  x: 'px',  // Pixel
  p: '%',   // Percent
  e: 'em',  // Relative to parent
  r: 'rem', // Relative to root element
  c: 'cm'   // Centimeter
}

// ClassnamePrefix-Property map
const omapClassProp = {
  p: 'padding',         // Padding All
  pt: 'padding-top',    // Padding Top
  pr: 'padding-right',  // Padding Right
  pb: 'padding-bottom', // Padding Bottom
  pl: 'padding-left',   // Padding Left
  m: 'margin',          // Margin  All
  mt: 'margin-top',     // Margin  Top
  mr: 'margin-right',   // Margin  Right
  mb: 'margin-bottom',  // Margin  Bottom
  ml: 'margin-left'     // Margin  Left
};

// Range of numbers to be used as property values:
const arrMagnitudeRange = [
  ...range(1, 20),
  ...range(25, 50, 5),
  ...range(60, 100, 10)
];

const visualDelim = minify ? '' : '\x20';

var css = '// Programmatically generated, inspect for potential errors.\n\n';

for (let sc in omapScaleUnit) {
  let unit = omapScaleUnit[sc];

  for (let cl in omapClassProp) {
    let prop = omapClassProp[cl];

    for (let mg of arrMagnitudeRange) {
      css +=
        `.${cl}${sc}${mg}${visualDelim}` +
        `{${visualDelim}${prop}:${visualDelim}${mg}${unit};${visualDelim}}\n`;
    }
  }
}

console.log(css);
```
