# [CSS Modules](https://github.com/css-modules/css-modules) for Meteor

**What are CSS Modules?**
"A CSS Module is a CSS file in which all class names and animation names are scoped locally by default." - CSS modules github

**Why use CSS Modules?**
In short, you can separate your CSS into components, just like you do with the HTML and JavaScript for your Blaze or React Templates.
Or, as stated on the main CSS modules page:

* No more conflicts.
* Explicit dependencies.
* No global scope.

## Installation

Install using Meteor's package management system:

```bash
meteor remove ecmascript
meteor add nathantreid:css-modules
```

In order to support the **import ... from** syntax, this plugin will need to handle your JS files. **This plugin includes the meteor ecmasscript compiler, so you still get all of the ES6 goodness!**
If you don't want the **import ... from** syntax for JavaScript files, you can just install the MSS processor. In this case you will need to use the alternative JS import syntax shown in the Usage section below.

```bash
meteor add nathantreid:css-modules-mss
```

## Usage

Because Meteor 1.2 doesn't allow build plugins to handle CSS files, you will need to use the .mss (Modular Style Sheet) extension.

***hello.mss***
``` css
.hello {
    composes: b from "./b.mss";
    color: red;
}
```

***b.mss***
``` css
.b {
    font-weight: bold;
}
```

***hello.js***
``` js
import * as styles from "{}/hello.mss";

Template.hello.helpers({
    styles: styles
});
```

***hello.html***
``` html
<template name="hello">
  <button class="{{styles.hello}}">Click Me</button>
</template>
```

***Alternative JS import syntax***
``` js
Template.hello.helpers({
    styles: CssModules.import("{}/hello.mss")
});
```

### Relative Imports
Relative imports are supported when using the **import ... from** syntax.
Given the structure:
```
root
- client
  - hello.js
  - hello.mss
```

you can do the following:

***hello.js***
``` js
import * as styles from "./hello.mss";

Template.hello.helpers({
    styles: styles
});
```


This will be converted to:
``` js
var styles = CssModules.import("{}/client/hello.mss");

Template.hello.helpers({
    styles: styles
});
```

## PostCSS Plugins
The following PostCSS plugins are used:

* postcss-modules-values
* postcss-modules-local-by-default
* postcss-modules-extract-imports
* postcss-modules-scope
* postcss-nested
* postcss-nested-props
* postcss-media-minmax
* postcss-color-hex-alpha
* postcss-pseudo-class-any-link
* postcss-selector-not

They can be referenced by the following variables:

* CssProcessor.values
* CssProcessor.localByDefault
* CssProcessor.extractImports
* CssProcessor.scope
* CssProcessor.mediaMinMax
* CssProcessor.nestedProps
* CssProcessor.nested
* CssProcessor.colorHexAlpha
* CssProcessor.anyLink
* CssProcessor.notSelector

The first four are **required** for basic CSS modules; the rest are plugins I like to use.
If you wish to remove their functionality, you can do so by creating a [config/css-modules.json](https://github.com/nathantreid/meteor-css-modules-test/blob/master/config/css-modules.json) file.

## History

* 11/2015: Implementation of CSS Modules for Meteor released
* 11/2015: Shortened auto-generated class names by excluding irrelevant path information
* 11/2015: Implemented source maps
* 11/2015: Added additional postcss plugins (nested, nested props, media min/max, colorHexAlpha, anyLink, and notSelector.

## Todo

* consider replacing Meteor's .css processor (replace standard-minifiers)
* allow custom selection of postcss plugins, similar to https://github.com/juliancwirko/meteor-postcss

## Acknowledgements
This plugin was developed using the CSS modules implementation found here: https://github.com/css-modules/css-modules-loader-core
