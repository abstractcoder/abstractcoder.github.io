---
title: Javascript Regex Pitfall
layout: post
---

I've run into a frustrating issue with several of my apps where a regular expression I'm using seems to periodically fail.

It turns out the issue has to do with enabling global search on the regular expression in conjunction with multiple calls to the test method. Every time the test method is called on a regular expression with global search it updates an internal index which points at the last match. Each subsequent call begins the test starting with the character proceeding the previous match. If there is no match after the previous match then the test returns false and the index is reset.

In the case where there is only one match in the string the test method will fail every other time. In the case of two more matches, calls to test will return true once for every match, then return false.

Here is some example code that illustrates this behavior [http://jsfiddle.net/abstractcoder/gx5yh/](http://jsfiddle.net/abstractcoder/gx5yh/)

### Code
```javascript
var output = "";

var test = "a";
var regex = new RegExp(test, "gi");
for (var i=0; i < 10; i++) {
    output += regex + ".test('" + test + "') is " + regex.test(test) + "\n";
}
output += "\n";

test = "aaa";
for (var i=0; i < 10; i++) {
    output += regex + ".test('" + test + "') is " + regex.test(test) + "\n";
}
output += "\n";

test = "a";
var regex = new RegExp(test, "i");

for (var i=0; i < 10; i++) {
    output += regex + ".test('" + test + "') is " + regex.test(test) + "\n";
}
console.log(output);
```

### Output

```
/a/gi.test('a') is true
/a/gi.test('a') is false
/a/gi.test('a') is true
/a/gi.test('a') is false
/a/gi.test('a') is true
/a/gi.test('a') is false
/a/gi.test('a') is true
/a/gi.test('a') is false
/a/gi.test('a') is true
/a/gi.test('a') is false

/a/gi.test('aaa') is true
/a/gi.test('aaa') is true
/a/gi.test('aaa') is true
/a/gi.test('aaa') is false
/a/gi.test('aaa') is true
/a/gi.test('aaa') is true
/a/gi.test('aaa') is true
/a/gi.test('aaa') is false
/a/gi.test('aaa') is true
/a/gi.test('aaa') is true

/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
/a/i.test('a') is true
```

The solution is to simply not specify global search when using a RegExp in conjunction with multiple calls to the test method.