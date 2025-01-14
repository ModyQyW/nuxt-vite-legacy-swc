# nuxt-vite-legacy-swc

Nuxt module to make your Nuxt3 app compatible with legacy browsers.

Uses [vite-plugin-legacy-swc](https://www.npmjs.com/package/vite-plugin-legacy-swc) and applies a number of hacks that Nuxt.js team [decided to avoid](https://github.com/nuxt/nuxt/issues/15464#issuecomment-1596895289).

Tested on Nuxt 3.4 to 3.8, and also on Nuxt 3.13.

## Quick Setup

Install:

```sh
npm install nuxt-vite-legacy-swc --save-dev
```

Add `nuxt-vite-legacy-swc` to the `modules` section of `nuxt.config.ts`:

```ts
export default defineNuxtConfig({
  modules: ["nuxt-vite-legacy-swc"],
  // Optionally, provide vite-plugin-legacy-swc options.
  // For example, for Chrome 49 you could use:
  legacy: {
    targets: ["chrome 49"],
    additionalLegacyPolyfills: [
      "mdn-polyfills/Element.prototype.getAttributeNames",
    ],
  },
});
```

## Caveats

### Upgrading

Nuxt and `vite-plugin-legacy-swc` are upgraded separately and the whole matrix is not well tested. The module may behave incorrectly with older Nuxt and newer `vite-plugin-legacy-swc` or vice versa.

Please report if you encounter issues; better yet, a PR adding the Github action testing matrix would be much appreciated.

### Legacy browsers that had just begin to support modules

The legacy build will be used for browsers that don't support `<script module>` which is enough most of the time.

However, this leaves incompatibility window for legacy browsers that **do** support modules but don't support modern features such as async generators (based on caniuse that would be e.g. Chrome 61-62). Vanilla `vite-plugin-legacy-swc` injects [special detection scripts](https://github.com/vitejs/vite/blob/535795a8286e4a9525acd2340e1d1d1adfd70acf/packages/plugin-legacy/src/snippets.ts) into SSR HTML, which this Nuxt module doesn't. PR's welcome!

## Development and Testing

Install Node 20, then:

```sh
corepack enable
pnpm i
pnpm dev:prepare
pnpm dev:build && pnpm dev:start
```

<http://localhost:3000> will run a Chrome 49-compatible version.
