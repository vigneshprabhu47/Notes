**1. List of invisible & zero-length characters (JavaScript character code + unicode string)**
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

**2. Simple date formats**
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
