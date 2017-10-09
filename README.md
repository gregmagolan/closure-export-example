## Closure export example

An example code that uses the `export { x, y as z } from 'some-module'` form.

Bundle fails with runtime error with https://github.com/google/closure-compiler/pull/2641 dist. Bundle succeeds with dist that includes this patch: https://github.com/ChadKillingsworth/closure-compiler/pull/2.

### Setup

* `yarn install`

### Build and run with PR2641

* `yarn build`
* `yarn serve` to serve up on port 8080 (error seen in console)
* `yarn start` to just run bundle with node to see error
* `yarn explore` to display the contents of the bundle source map

The runtime error seen is the following:

```
ReferenceError: module$src$bar is not defined
```

### Build and run with PR2641 + patches

* `yarn build-with-patches`
* `yarn serve` to serve up on port 8080 (error seen in console)
* `yarn start` to just run bundle with node to see error
* `yarn explore` to display the contents of the bundle source map

## Entry point issue

The entry point issue fixed with can also be seen in this repo.

`closure.conf` has the entry point set to `--entry_point=src/bootstrap`. If the entry point is changed to `--entry_point=./built/src/bootstrap` and built with PR2641 you get an invalid bundle (since the entry point is not found) that looks like this:

```
module$src$foo.Foo.test();module$src$foo.Bar2.test();
```

and a runtime error:

```
ReferenceError: module$src$foo is not defined
```