---
description: "Configuration options for electric-cli tasks."
icon: "gear"
layout: "docs"
title: "Configuration"
weight: 5
---

<article id="registering">

## electric.config.js

All options can be set in the `electric.config.js` file located in the root of
your project. This file must export an Object, or a
function that returns an Object.

```text/javascript
module.exports = {
	pathDest: 'build'
};

// or

module.exports = function() {
	return {
		pathDest: 'build'
	};
};
```

</article>

<article id="options">

## Options

### codeMirrorLanguages

- Type: `Array<languageName>`
- Default: `['xml', 'css', 'javascript']`

An Array of langauge names for syntax highlighting.
See [CodeMirror](https://codemirror.net/mode/index.html) for a list of available
languages.

### codeMirrorTheme

- Type: `String`
- Default: `'dracula'`

Theme to be used by CodeMirror.
See [CodeMirror](https://codemirror.net/demo/theme.html) for a list of available
themes.

### deployOptions

- Type: `Object`
- Default: `{lb} branch: 'wedeploy' {rb}`

Configuration options used by the `deploy` command. See [gulp-gh-pages](https://www.npmjs.com/package/gulp-gh-pages#ghpages-options-) for further configuration options.

### frontMatterHook

- Type: `function`

Allows modifications of your project's front-matter before it's passed to the
templates.

Example:

```text/javascript
module.exports = {
	frontMatterHook: function(data) {
		data.someValue = 'Hello, World!';

		return data;
	}
};
```

In this example the property `someValue` will be accessible via
the `$site` parameter in your pages/templates.

```markdown
---
description: "Page description."
layout: "docs"
title: "Page"
---

{$site.someValue}
```

### markdownOptions

- Type: `Object`

An Object Literal containing configuration options
for [Remarkable](https://github.com/jonschlinkert/remarkable) which is
used to render Markdown files.

Example:

```text/javascript
module.exports = {
	markdownOptions: {
		breaks: true
	}
};
```

See [Remarkable's documentation](https://github.com/jonschlinkert/remarkable#options) for
list of options.

### markdownRenderer

- Type: `function|Remarkable`

Allows complete customization over the markdown renderer. Value can either be
instance of [Remarkable](https://github.com/jonschlinkert/remarkable) or a
function which receives the default instance of `Remarkable` as an argument.

Example:

```text/javascript
// New Remarkable instance
module.exports = {
	markdownRenderer: new Remarkable()
};

// Function
module.exports = {
	markdownRenderer: function(md) {
		md.core.ruler.enable([
			'abbr'
		]);

		return md;
	}
};
```

### pathDest

- Type: `String`
- Default: `dist`

The path that generated files are placed in.

Example:

```text/javascript
module.exports = {
	pathDest: 'build'
};
```

Now all generated files will be placed in the `build` directory.

### pathSrc

- Type: `String`
- Default: `src`

The path where all source files are located.

Example:

```text/javascript
module.exports = {
	pathSrc: 'web'
};
```

Now `electric` will look inside the `web` directory for all source files.

### metalComponents

- Type: `Array<String>`

Array of `npm` modules that expose Metal components. These also must be added
as `npm` dependencies in your package.json.

Example:

```text/javascript
module.exports = {
	metalComponents: ['electric-components']
};
```

The components found in the `electric-components` package will now be available
to all `soy` files in your project.

Note that every package listed in `metalComponents` must also be listed as a dependency
in the project's `package.json`.

### sassOptions

- Type: `Object`

Config object passed to [gulp-sass](https://www.npmjs.com/package/gulp-sass).

Example:

```text/javascript
module.exports = {
	sassOptions: {
		includePaths: ['node_modules/some-package/scss']
	}
};
```

### staticSrc

- Type: `Array<String>`

Array of glob patterns for static files found in the
`options.pathSrc` directory. These glob patterns target anything that isn't
part of the build system (pages, layouts, styles, etc.).

Note: it is not recommended to overwrite this property.

### uglifyBundle

- Type: `boolean`
- Default: `false`

Determines if bundle files are both minified and uglified.

When using `electric-cli`, this option defaults to `true` during
the `deploy` task, and defaults to `false` during all others.

Warning: enabling this option can significantly increase build times.

### vendorSrc

- Type: `Array<String>`
- Default: `[]`

Array of glob patterns for .css and .js files that should be included in
the `<head>` of your project.

Example:

```text/javascript
module.exports = {
	vendorSrc: ['node_modules/some-project/src/min.js']
};
```

</article>
