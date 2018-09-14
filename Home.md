## [Office UI Fabric React](http://dev.office.com/fabric)

##### The React-based front-end framework for building experiences for Office and Office 365.

[![npm version](https://badge.fury.io/js/office-ui-fabric-react.svg)](https://badge.fury.io/js/office-ui-fabric-react)
[![Build Status](https://travis-ci.org/OfficeDev/office-ui-fabric-react.svg?branch=master)](https://travis-ci.org/OfficeDev/office-ui-fabric-react)

Fabric React is a responsive, mobile-first collection of robust components designed to make it quick and simple for you to create web experiences using the Office Design Language.

### Using Fabric React

Integrating components into your project depends heavily on your setup. The recommended setup is to use a bundler such as Webpack which can resolve NPM package imports in your code and can bundle the specific things you import.

Within an npm project, you should install the package and save it as a dependency:

```
npm install --save office-ui-fabric-react
```

Once installed, you will be able to use the components within your own project.

```js
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { PrimaryButton } from 'office-ui-fabric-react/lib/Button';

ReactDOM.render(
  <PrimaryButton>
    I am a button.
  </PrimaryButton>,
  document.body.firstChild
);
```

To lean more select a topic in the wiki sidebar, or start with our [Fabric setup page](Getting-Set-Up).

## Browser support

Fabric React supports many commonly used browsers. See the [browser support doc](Browser-Support) for more information.

## Contribute to Fabric React

Please take a look at our [contribution guidelines](Contributing) for more info. Also read [Contribute Bug fixes](Bug-Fixes) and [Contribute New component](New-Components).

## Testing

For testing see our [testing documentation](Testing).

## Advanced usage

For advanced usage including info about module vs. path-based imports, using an AMD bundler like Require, and deployment features, see our [advanced documentation](Advanced-Usage).


## Licenses

All files on the Office UI Fabric React GitHub repository are subject to the MIT license. Please read the License file at the root of the project.

Usage of the fonts and icons referenced in Office UI Fabric is subject to the terms of the [assets license agreement](http://aka.ms/fabric-assets-license).

## Changelog

We use [GitHub Releases](https://github.com/blog/1547-release-your-software) to manage our releases, including the changelog between every release. View a complete list of additions, fixes, and changes on the [releases](https://github.com/OfficeDev/office-ui-fabric-react/releases) page.

- - -

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
