# Synced with [`microbundle@v0.12.4`](https://github.com/developit/microbundle/compare/master...eik-lib:main?w=1)

[Why did we fork microbundle?](https://eik.dev/docs/mapping_bundling#why-we-forked-microbundle)

This fork is currently serving two purposes:

1. Support import maps using the eik rollup plugin and other functionality that's only possible by forking, as microbundle doesn't allow extending this from the outside in its public API.
2. Provide optimal defaults for how FINN.no is using Eik.

We see the error of our ways and in the spirit of making upstream contributions to microbundle as easy as possible, we'll refactor this fork to only serve purpose 1. We'll create a new package on our GHE for FINN.no that will wrap this fork and implement the features that are useful to us but perhaps not to non-FINN use cases.
The long term plan is that we want to delete this fork and make our internal microbundle wrapper simply wrap microbundle directly instead of our fork.

## [Usage instructions](https://eik.dev/docs/mapping_bundling)

This fork isn't intended to stay around, once we microbundle supports supports import maps we'll update the docs in the above link to point to microbundle instead of this fork.

## Functionality we intend to contribute upstream

These are the features that are the primary reason for our fork, and that we intend to contribute upstream in order to allow us to stop maintaining this fork and return to using microbundle directly.

- support import mapping ESM.
- CSS import mapping
- postcss-import plugin.
- `@rollup/plugin-replace` are added and setup to replace `process.env.NODE_ENV`, a behavior which browserify and webpack does by default.
- the `iife` rollup format is enabled, used by ie11 fallbacks.

## Functionality that will be moved to our internal abstraction

These features are defaults and other options that make sense to the way FINN.no applications and libraries are set up. We recognize that these new defaults don't make sense to the majority of microbundle users. Thus they're all going away soon from this fork and set in an internal abstraction instead.

- CSS modules (`--css-modules`) are disabled by default, originally it's enabled by default.
- `babel-plugin-react-require` is added and setup, however the new jsx-runtime transform in `@babel/preset-react` introduced in `react@v17` makes this plugin no longer necessary.
- using `-f modern` or `-f iife` will also behave as if you set `--externals none` as the `iife` fallback bundle always needs to include everything. And the `modern` formats are using the import map plugin anyways.
- `--jsx` is changed from `h` to `React.createElement` and is ignored. The same is true for `--jsxFragment`, which is `Fragment` by default and configurable. In this fork, it's not configurable and hardcoded to `React.Fragment`.
- The default value for formats `-f` is changed from `modern,es,cjs,umd` to `modern,iife`.
- Originally the `--output` option didn't have a default value, the fork however sets it to `dist`.
