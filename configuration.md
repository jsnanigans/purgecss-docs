# Configuration

Purgecss has  a list of options that allow you to customize its behavior. Customization can improve the performance and efficiency of Purgecss. You can create a configuration file with the following options.

## Configuration file

The configuration file is a simple JavaScript file. By default, the JavaScript API will look for `purgecss.config.js`.

```js
module.exports = {
    content: ['index.html'],
    css: ['style.css']
}
```

You can then use Purgecss with the file:

```js
const purgecss = new Purgecss()
// or use the path to the file as the only parameter
const purgecss = new Purgecss('./purgecss.config.js')
```

## Options

```
{
  content: Array<string | RawContent>,
  css: Array<string>,
  extractors?: Array<ExtractorsObj>,
  whitelist?: Array<string>,
  whitelistPatterns?: Array<RegExp>,
  stdin?: boolean,
}
```

* #### content

You can specify content that should be analyzed by Purgecss with an array of filenames or [globs](https://github.com/isaacs/node-glob/blob/master/README.md#glob-primer). The files can be HTML, Pug, Blade, etc.

```js
new Purgecss({
    content: ['index.html', `**/*.js`, '**/*.html', '**/*.vue'],
    css: [`css/app.css`]
}
```

Purgecss also works with raw content. To do this, you need to pass an object with the `raw` and `extension` properties instead of a filename.

```js
new Purgecss({
    content: [
        {
            raw: '<html><body><div class="app"></div></body></html>',
            extension: 'html'
        },
        `**/*.js`, '**/*.html', '**/*.vue'],
    css: [`css/app.css`]
}
```

* #### extractors

Purgecss can be adapted to suit your needs. If you notice a lot of unused CSS is not being removed, you might want to use a custom extractor.

```js
new Purgecss({
    content: ['index.html', `**/*.js`, '**/*.html', '**/*.vue'],
    css: [`css/app.css`],
    extractors: {
        extractor: class {
            static extract(content) {
                content.match(/a-Z/) || []
            }
        },
        extension: ['html', 'blade']
    }
}
```

More information about extractors [here](/extractors.md).

* #### whitelist

You can whitelist selectors to stop Purgecss from removing them from your CSS. This can be accomplished with the options `whitelist` and `whitelistPatterns`.

```js
const purgecss = new Purgecss({
    content: [], // content
    css: [], // css
    whitelist: ['random', 'yep', 'button']
})
```

In the example, the selectors `.random`, `#yep`, `button` will be left in the final CSS.

* #### whitelistPatterns

You can whitelist selectors based on a regular expression with `whitelistPatterns`.

```js
const purgecss = new Purgecss({
    content: [], // content
    css: [], // css
    whitelistPatterns: [/red$/]
})
```

In the example, selectors ending with `red` such as `.bg-red` will be left in the final CSS.

* #### stdin

stdin allows you to set the CSS code itself in the CSS options directly.

```js
const purgecss = new Purgecss({
    content: ['index.html', `**/*.js`, '**/*.html', '**/*.vue'],
    css: ['html, body { width: 100%; height: 100%} fieldset { border: none; }']
})
```

* #### keyframes \(default: false\)

If you are using a CSS animation library such as animate.css, you can remove unused keyframes by setting the `keyframes` option to `true`.

```js
new Purgecss({
    content: ['index.html', `**/*.js`, '**/*.html', '**/*.vue'],
    css: [`css/app.css`],
    keyframes: true
}
```



