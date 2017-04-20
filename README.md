# PostCSS Sequence [![Build Status][ci-img]][ci]

Manage the consistent scale and proportion of your design using custom units based on numerical sequences.

[PostCSS]: https://github.com/postcss/postcss
[ci-img]:  https://travis-ci.org/sevenupcan/postcss-sequence.svg
[ci]:      https://travis-ci.org/sevenupcan/postcss-sequence

# Usage

Create custom units by defining an abbreviation and the pattern to generate the `n` index from a numerical sequence.
```
units: [
    { 
        abbr: 'gu', 
        pattern: 'n * 4',
    }]
```

Example:
```
div {
    padding: 4gu;
}
```

Outputs:
```
div {
     padding: 16px; // (4 * 4)
}
```

# Setup

```
npm install postcss-sequence
```

Set the options for your units in the plugin
```
units: [
    { 
        abbr: 'gu',
        pattern: 'n * 4',
    }]
```

# Options

- `abbr`: A `string`  with the abbreviation of the name for your unit.  For more complex units you can use a number identifier like so `<g>` where `g` is the variable used in your pattern. If you require something unique you can also use a `regex`.
- `position`: Position of where the abbreviation is placed. To the end by default. Choose from `'start'`, `'middle'` or `'end'`.  
- `pattern`: Pattern used to generate the `n` index from a numerical sequence. If using a regex or middle position affix you can use lowercase letters starting from `a` to reference groups in in the order they were captured. See example below.
- `output`:  Optional,`'px'` used by default.


# Various examples

### Grid units

A grid unit based on a grid of 4px
```js
units: [
    { 
        abbr: 'gu',
        pattern: 'n * 4',
    }]
```

Usage:
```css
div {
	padding: 4gu;
	margin: 4gu 2gu;
}
```

Outputs:
```css
div {
	padding: 16px;
	margin: 16px 8px;
}
```

### Font scale

Setting the font based on a scale using golden ratio
```js
units: [
    {
        abbr: 'x',
        pattern: 'n * pow(16, 1.68)',
        output: 'em'
    }]
```

Usage:
```css
h1 {
	font-size: 5x;
}
h2 {
	font-size: 4x;
}
h3 {
	font-size: 2x;
}
```

### Feet and inches

Writing in feet and inches
```js
units: [
    {
        abbr: /([0-9]+)\'([0-9]+)\"/,
        pattern: '((a * 12) + b ) * 10',
        output: 'px'
    }]
```

Usage:
```
div {
	width: 4'2";
}
```

Ouputs:
```
div {
	width: 500px;
}
```

### Fractions

Using fractions for percentages
```js
units: [
    {
        abbr: '<n>/<d>',
        pattern: 'n / d',
        output: '%'
    }]
```

Usage:
```
div {
	width: 1/4;
}
```

Ouputs:
```
div {
	width: 25%;
}
```

### Automatic rounding

Automatically round pixels to the nearest ten pixels
```js
units: [
    {
        abbr: 'px',
    	pattern: 'round (n / 10) * 10'
    }]
```

Usage:
```
div {
	width: 26px;
}
```

Outputs:
```
div {
	width: 30px;
}
```

### Fibonacci number

Selecting a number from the fibonacci sequence
```js
units: [
    {
	    abbr: 'f',
	    position: 'start',
	    pattern: 'floor (PHI ^ n / sqrt 5 + 0.5)'
    }]
```

Usage:
```
div {
	width: f6;
	padding: f3 f4;
}
```
Outputs:
```
div {
	font-size: 24px;
	padding: 4.8px;
}
```

See [PostCSS] docs for examples for your environment.