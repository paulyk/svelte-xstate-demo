# Note

Demo of svelte and xstate.

## rollup

In order to get this working you will need to install `@rollup/plugin-replace`, and the configure rollup.config.js to use it.

```
npm i -D @rollup/plugin-replace
```

Place after `plugins: [ svelte()... ]`
```
replace({
    "process.env.NODE_ENV": production
        ? JSON.stringify("production")
        : JSON.stringify("development")
})
```