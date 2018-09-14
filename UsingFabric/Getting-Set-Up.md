## Prerequisites

### Node.js and npm

- Install **Node.js LTS 8** (v.8.12.x as of writing) from the **[Node.js website](https://nodejs.org/en/)**. This installation will include NPM.

>You can check your node and npm version by running `node -v` and `npm -v` respectively.

### Git
- Install **[Git](https://git-scm.com/)**.

> This isn't required to use Fabric, but is necessary to checkout any demo code

### Code Editor

- For code editing we like **[Visual Studio Code ](https://code.visualstudio.com/)**



## Integration Into Your Project

Integrating components into your project depends heavily on your setup. The recommended setup is to use a bundler such as [Webpack](https://webpack.js.org/) which can resolve NPM package imports in your code and can bundle the specific things you import. 

Within an npm project, you should install the package and save it as a dependency:

```
npm install --save office-ui-fabric-react
```

Once installed, you will be able to use the components within your own project. 

```js
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { PrimaryButton } from 'office-ui-fabric-react';

ReactDOM.render(
  <PrimaryButton>
    I am a button.
  </PrimaryButton>,
  document.body.firstChild
);
```

### Advanced Use Cases
If you not using Webpack 4 or greater, or are using Browserify, AMD or Server Side Rendering(SSR), check out our [Advanced Use Cases](Advanced-Use-Cases) section.


## Next Steps

Check out our [Example App](Example-App) if you want step by step instructions on creating a new app using Fabric components.

If you already have an app, jump to [Custom Themes and Component Styles](Custom-Themes-and-Component-Styles)



