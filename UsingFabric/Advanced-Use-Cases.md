## Notes on module vs path-based imports

We highly recommend using Webpack 4+. If you use Webpack 4 or Rollup.js, you can take advantage of tree-shaking to create smaller bundles. This allows you to imports the parts you need in a natural way:

```typescript
import { Button } from 'office-ui-fabric-react';
```

This import will include the Button, plus the dependent modules. It should not include any modules that are unrelated.

## For Webpack < 4 or Browserify

If you are using a non-tree-shaking bundler, you may accrue larger bundle sizes by importing from the package entry point `office-ui-fabric-react`. By default bundlers will include every module referenced from that entry point, even if you don't use them.

To address this friction, we also offer "top level imports" which help scope the graph to only the component and its dependencies.

```typescript
import { Button } from 'office-ui-fabric-react/lib/Button';
import { Dropdown } from 'office-ui-fabric-react/lib/Dropdown';
import { List } from 'office-ui-fabric-react/lib/List';
```

For a full list of top level imports, see the source here:

https://github.com/OfficeDev/office-ui-fabric-react/tree/master/packages/office-ui-fabric-react/src

## Using an AMD bundler like r.js

If your project relies on AMD modules, they are dropped in the lib-amd folder. You will need to set up your bundler to handle the imports correctly. This may require you to symlink or copy the folder into your pre-bundle location.

## Server-side rendering

If you need to render Fabric components on the server side in a node environment, there is a way to do this. The basic idea is that you need to tell the styles loader to pipe styles into a variable, which you can later use to inject into your page. Example:

```ts
import { configureLoadStyles } from '@microsoft/load-themed-styles';

// Store registered styles in a variable used later for injection.
let _allStyles = '';

// Push styles into variables for injecting later.
configureLoadStyles((styles: string) => {
  _allStyles += styles;
});

import * as React from 'react';
import * as ReactDOMServer from 'react-dom/server';
import { Button } from 'office-ui-fabric-react/lib/Button';

let body = ReactDOMServer.renderToString(<Button>hello</Button>);

console.log(
  `
  <html>
  <head>
    <style>${_allStyles}</style>
  </head>
  <body>
    ${body}
  </body>
  </html>
  `
);
```

Note: we are evaluating a more robust theming and style loading approach, which will allow a much more flexible server rendering approach, so this syntax may be simplified in the future.

### Browserless Testing

In unit or end-to-end tests that run in an SSR-like (non-browser) environment such as Node, you'll need to disable style loading.

```typescript
import { initializeIcons } from 'office-ui-fabric-react/lib/Icons';
initializeIcons('dist/');

// Configure load-themed-styles to avoid registering styles.
let themeLoader = require('@microsoft/load-themed-styles');
themeLoader.configureLoadStyles(styles => {
  // noop
});

// Set ssr mode to true, and rtl to false.
let library = require('office-ui-fabric-react/lib/Utilities');
library.setSSR(true);
library.setRTL(false);

// Assume a large screen.
let responsiveLib = require('office-ui-fabric-react/lib/utilities/decorators/withResponsiveMode');
responsiveLib.setResponsiveMode(responsiveLib.ResponsiveMode.large);
```

You'll also want to mock out requiring `.scss` files.
In Jest:

```js
  moduleNameMapper: {
    // jest-style-mock.js should just contain module.exports = {};
    '\\.(scss)$': path.resolve(__dirname, 'jest-style-mock.js'),
  }
```