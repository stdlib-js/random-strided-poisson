<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# Poisson Random Numbers

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Fill a strided array with pseudorandom numbers drawn from a [Poisson][@stdlib/random/base/poisson] distribution.

<section class="installation">

## Installation

```bash
npm install @stdlib/random-strided-poisson
```

Alternatively,

-   To load the package in a website via a `script` tag without installation and bundlers, use the [ES Module][es-module] available on the [`esm`][esm-url] branch (see [README][esm-readme]).
-   If you are using Deno, visit the [`deno`][deno-url] branch (see [README][deno-readme] for usage intructions).
-   For use in Observable, or in browser/node environments, use the [Universal Module Definition (UMD)][umd] build available on the [`umd`][umd-url] branch (see [README][umd-readme]).

The [branches.md][branches-url] file summarizes the available branches and displays a diagram illustrating their relationships.

To view installation and usage instructions specific to each branch build, be sure to explicitly navigate to the respective README files on each branch, as linked to above.

</section>

<section class="usage">

## Usage

```javascript
var poisson = require( '@stdlib/random-strided-poisson' );
```

#### poisson( N, lambda, sl, out, so )

Fills a strided array with pseudorandom numbers drawn from a [Poisson][@stdlib/random/base/poisson] distribution.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
poisson( out.length, [ 2.0 ], 0, out, 1 );
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **lambda**: rate parameter.
-   **sl**: index increment for `lambda`.
-   **out**: output array.
-   **so**: index increment for `out`.

The `N` and stride parameters determine which strided array elements are accessed at runtime. For example, to access every other value in `out`,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

poisson( 3, [ 2.0 ], 0, out, 2 );
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Initial array:
var lambda0 = new Float64Array( [ 2.0, 2.0, 2.0, 2.0, 2.0, 2.0 ] );

// Create offset view:
var lambda1 = new Float64Array( lambda0.buffer, lambda0.BYTES_PER_ELEMENT*3 ); // start at 4th element

// Create an output array:
var out = new Float64Array( 3 );

// Fill the output array:
poisson( out.length, lambda1, -1, out, 1 );
```

#### poisson.ndarray( N, lambda, sl, ol, out, so, oo )

Fills a strided array with pseudorandom numbers drawn from a [Poisson][@stdlib/random/base/poisson] distribution using alternative indexing semantics.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
poisson.ndarray( out.length, [ 2.0 ], 0, 0, out, 1, 0 );
```

The function has the following additional parameters:

-   **ol**: starting index for `lambda`.
-   **oo**: starting index for `out`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the offset parameters support indexing semantics based on starting indices. For example, to access every other value in `out` starting from the second value,

```javascript
var out = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

poisson.ndarray( 3, [ 2.0 ], 0, 0, out, 2, 1 );
```

#### poisson.factory( \[options] )

Returns a function for filling strided arrays with pseudorandom numbers drawn from a [Poisson][@stdlib/random/base/poisson] distribution.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var random = poisson.factory();
// returns <Function>

// Create an array:
var out = new Float64Array( 10 );

// Fill the array with pseudorandom numbers:
random( out.length, [ 2.0 ], 0, out, 1 );
```

The function accepts the following `options`:

-   **prng**: pseudorandom number generator for generating uniformly distributed pseudorandom numbers on the interval `[0,1)`. If provided, the function **ignores** both the `state` and `seed` options. In order to seed the underlying pseudorandom number generator, one must seed the provided `prng` (assuming the provided `prng` is seedable).
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators. Setting this option to `true` ensures that an underlying generator has exclusive control over its internal state. Default: `true`.

To use a custom PRNG as the underlying source of uniformly distributed pseudorandom numbers, set the `prng` option.

```javascript
var Float64Array = require( '@stdlib/array-float64' );
var minstd = require( '@stdlib/random-base-minstd' );

var opts = {
    'prng': minstd.normalized
};
var random = poisson.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 2.0 ], 0, out, 1 );
```

To seed the underlying pseudorandom number generator, set the `seed` option.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var opts = {
    'seed': 12345
};
var random = poisson.factory( opts );

var out = new Float64Array( 10 );
random( out.length, [ 2.0 ], 0, out, 1 );
```

* * *

#### random.PRNG

The underlying pseudorandom number generator.

```javascript
var prng = poisson.PRNG;
// returns <Function>
```

#### poisson.seed

The value used to seed the underlying pseudorandom number generator.

```javascript
var seed = poisson.seed;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = poisson.factory({
    'prng': minstd
});
// returns <Function>

var seed = random.seed;
// returns null
```

#### poisson.seedLength

Length of underlying pseudorandom number generator seed.

```javascript
var len = poisson.seedLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = poisson.factory({
    'prng': minstd
});
// returns <Function>

var len = random.seedLength;
// returns null
```

#### poisson.state

Writable property for getting and setting the underlying pseudorandom number generator state.

```javascript
var state = poisson.state;
// returns <Uint32Array>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = poisson.factory({
    'prng': minstd
});
// returns <Function>

var state = random.state;
// returns null
```

#### poisson.stateLength

Length of underlying pseudorandom number generator state.

```javascript
var len = poisson.stateLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = poisson.factory({
    'prng': minstd
});
// returns <Function>

var len = random.stateLength;
// returns null
```

#### poisson.byteLength

Size (in bytes) of underlying pseudorandom number generator state.

```javascript
var sz = poisson.byteLength;
// returns <number>
```

If the `factory` method is provided a PRNG for uniformly distributed numbers, the associated property value on the returned function is `null`.

```javascript
var minstd = require( '@stdlib/random-base-minstd-shuffle' ).normalized;

var random = poisson.factory({
    'prng': minstd
});
// returns <Function>

var sz = random.byteLength;
// returns null
```

</section>

<!-- /.usage -->

<section class="notes">

* * *

## Notes

-   If `N <= 0`, both `poisson` and `poisson.ndarray` leave the output array unchanged.
-   Both `poisson` and `poisson.ndarray` support array-like objects having getter and setter accessors for array element access.

</section>

<!-- /.notes -->

<section class="examples">

* * *

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var zeros = require( '@stdlib/array-zeros' );
var zeroTo = require( '@stdlib/array-zero-to' );
var logEach = require( '@stdlib/console-log-each' );
var poisson = require( '@stdlib/random-strided-poisson' );

// Specify a PRNG seed:
var opts = {
    'seed': 1234
};

// Create a seeded PRNG:
var rand1 = poisson.factory( opts );

// Create an array:
var x1 = zeros( 10, 'float64' );

// Fill the array with pseudorandom numbers:
rand1( x1.length, [ 2.0 ], 0, x1, 1 );

// Create another function for filling strided arrays:
var rand2 = poisson.factory( opts );
// returns <Function>

// Create a second array:
var x2 = zeros( 10, 'generic' );

// Fill the array with the same pseudorandom numbers:
rand2( x2.length, [ 2.0 ], 0, x2, 1 );

// Create a list of indices:
var idx = zeroTo( x1.length, 'generic' );

// Print the array contents:
logEach( 'x1[%d] = %.2f; x2[%d] = %.2f', idx, x1, idx, x2 );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/random-base/poisson`][@stdlib/random/base/poisson]</span><span class="delimiter">: </span><span class="description">Poisson distributed random numbers.</span>
-   <span class="package-name">[`@stdlib/random-array/poisson`][@stdlib/random/array/poisson]</span><span class="delimiter">: </span><span class="description">create an array containing pseudorandom numbers drawn from a Poisson distribution.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2025. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-strided-poisson.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-strided-poisson

[test-image]: https://github.com/stdlib-js/random-strided-poisson/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-strided-poisson/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-strided-poisson/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-strided-poisson?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-strided-poisson.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-strided-poisson/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-strided-poisson/tree/deno
[deno-readme]: https://github.com/stdlib-js/random-strided-poisson/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/random-strided-poisson/tree/umd
[umd-readme]: https://github.com/stdlib-js/random-strided-poisson/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/random-strided-poisson/tree/esm
[esm-readme]: https://github.com/stdlib-js/random-strided-poisson/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/random-strided-poisson/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-strided-poisson/main/LICENSE

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/random/base/poisson]: https://github.com/stdlib-js/random-base-poisson

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32

<!-- <related-links> -->

[@stdlib/random/array/poisson]: https://github.com/stdlib-js/random-array-poisson

<!-- </related-links> -->

</section>

<!-- /.links -->
