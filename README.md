# vue-tsc-pnpm

This is a little demo project that shows that TypeScript config resolution is broken for `pnpm` workspace packages in `vue-tsc`.

Just run `pnpm install` and `pnpm check` and the latter should give you an error like this:


```
pnpm check

> @ check […]/vue-tsc-pnpm
> vue-tsc --noEmit

Error: Cannot find module 'dschmidt-tsconfig'
[ … stacktrace … ]
```

By adding the following lines
```typescript
// Workaround: https://github.com/pnpm/pnpm/issues/5237
if (process?.env?.npm_config_user_agent?.startsWith('pnpm/')) {
    extendsPath = `./node_modules/${extendsPath}/package.json`;
}
```
to line 71f. of `node_modules/.pnpm/@volar+vue-language-core@1.3.0/node_modules/@volar/vue-language-core/out/utils/ts.js` you can make it work - just like with global packages.
