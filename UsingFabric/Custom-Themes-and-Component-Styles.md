# Custom Themes
This document describes theming at a high level. For low level technical details, follow the links.


## What is theming?

Theming is a mechanism by which a consistent look and feel can be applied to all the components on a page. For now, this means sharing a color scheme across the entire page.


## What's in a theme?
A theme is defined by a simple collection of variables (which we call slots) with string values.
For example, `themePrimary` by default is `"#0078d4"`, but it could easily be `"rgba(0, 120, 212)"`.


## What are theme slots?

There are two kinds of theme slots: Fabric palette slots and semantic slots.

Fabric palette slots are documented in [IPalette.ts](https://github.com/OfficeDev/office-ui-fabric-react/blob/master/packages/styling/src/interfaces/IPalette.ts).
These are descriptive slots, it gives you an idea of what the color is, but you decide how to use it.
Most slots with color names (besides `white` and `black`), such as `yellow` and `yellowLight`, will remain a shade of yellow in all themes, useful in cases where color has meaning (such as for errors and warnings).
Customizing Fabric palette slots allows broad-stroke changes to the overall color scheme.

Semantic slots are documented in [ISemanticColors.ts](https://github.com/OfficeDev/office-ui-fabric-react/blob/master/packages/styling/src/interfaces/ISemanticColors.ts).
These, on the other hand, are prescriptive slots, each slot having an intended use.
This allows more targeted customizations.
For example, you could change the light gray color of a disabled checkbox without affecting the light gray background used by list item hover/selection state, even though they share the same color, because they use different slots.
This wouldn't be possible with Fabric palette slots.
We highly recommend using semantic slots wherever possible.


## How do I make a theme?

Check out the [Theme Generator](https://developer.microsoft.com/en-us/fabric#/styles/themegenerator) tool to quickly create a custom theme.
Currently only Fabric palette slots are generated. Semantic slots can be manually added to the resulting output.

You can define just the subset of slots you want to customize.
For instance, you could overwrite all the `neutral*`s with shades of red.
Anything using one of those Fabric palette slots will become reddish.

Semantic slots usually "inherit" from a Fabric palette slot.
For example, the `bodyText` semantic slot, if uncustomized, will always pick up the color from the Fabric palette slot `neutralPrimary`.
You can see these default assignments in [theme.ts](https://github.com/OfficeDev/office-ui-fabric-react/blob/master/packages/styling/src/styles/theme.ts) in `_makeSemanticColorsFromPalette()`.


## What happens when you theme?

At a high level, there is a theme definition, which defines the desired color scheme. The theming module, which does the actual work, parses this theme definition, then updates any registered CSS to match the given color scheme.

Inside the registered CSS are theme tokens that the theming module keeps track of. These theme tokens are basically variables, whose values the theming module changes based on the provided theme definition.


## What does the theming?

We have two theming modules that can apply a theme.
The first operates on raw CSS, the second manages styles defined in code that are not put onto the page until needed.

### `load-themed-styles` package

The old way, which most of our components still use, uses the [load-themed-styles package](https://github.com/Microsoft/web-build-tools/tree/292582a72afbcff6409c89bfb258ca6fa65f27b3/libraries/load-themed-styles).
This is a backwards-compatible method of theming that operates on raw CSS, such as CSS written by hand or compiled from SASS or LESS.
The raw CSS uses theming tokens, which are strings, as variables, in place of actual colors.
The module will find these tokens and replace them with the value of the corresponding slot as the theme changes.
All styles must also be registered through this module instead of being put directly on the page.
See the documentation for more details on implementation.

### `styling` package
The new and recommended way is the [styling package](Component-Styling) in this repo.
Styles are defined in a JSON format and managed in code, avoiding issues like specificity and providing a typesafe surface for customization.
The theme itself gets passed to the component. As the theme changes, the state of the component changes as well, and it will modify its own CSS to match.

This is the recommended path forward for new components, but it does have quite a learning curve.
[todo: link to new documentation about conversion process]

The `loadTheme()` call in the `@uifabric/styling` package will automatically invoke `loadTheme()` from `load-themed-styles` as well.
Thus, new environments should implement theming with the `@uifabric/styling` package regardless of whether they intend to do styles-in-code or styles with a CSS preprocessor.
Additionally, the `@uifabric/styling` package contains logic for defining default and fallback values for certain theme slots.


## Who applies the theme?

The host app is responsible for getting the theme and then applying it to the page by calling `loadTheme()`.
This can be called multiple times to dynamically change the theme.



EXAMPLES!!!


# Styling A Component

## `styles` prop

Unlike a `style` prop that only applies styles to the root component, the `styles` prop allows you to control the styling of every part of a component. The root, the children, and even sub components. You can use this for one off component customizations, or you can create a brand new component with these custom styles. In fact, all of the variants in Fabric are just components built by passing in different values for `styles`.

A component consists of dom elements, or "areas". Each of the areas should be targetable for styling.

### Object based Styling

```tsx
// Define styling, split out styles for each area.
const styles: IComponentStyles {
  root: { /* styles */ },
  child1: ['className', { /* styles */ }],
  child2: { /* styles */ }
  subComponentStyles: {
    subComponent: {
      root: { /* styles */ },
      child1: { /* styles */ },
    }
  }
}
```

__Example__

```tsx
const theme = getTheme();
const styles = {
  root: [
    {
      background: theme.palette.themePrimary,
      display: 'none',
      selectors: {
        ':hover': {
          background: theme.palette.themeSecondary,
        },
        '&.isExpanded': {
            display: 'block'
        },
        '&:hover .childElement': {
            color: 'white' 
        }
      }
    }
  ]
};
```

### Function based Styling

The styling applied to each area may depend on the state of the component as well as the contextual theme settings. So you should also be able to define styles as a function of these inputs:

```tsx
// Take in styling input, split out styles for each area.
const styles = (props: IComponentStyleProps): IComponentStyles => {
  return {
    root: { /* styles */ },
    child1: ['className', { /* styles */ }],
    child2: ['className', props.someBoolean && { /* styles */ }],
    subComponentStyles: {
      subComponent: (subProps:ISubComponentStyleProps) => {
        const { theme, disabled, hasBoolean } = props; // parent props are available in subComponents
        const { required, hasOtherBoolean } = subProps;
        return ({
          root: { /* styles */ },
          child1: { /* styles */ },
        });
      }
    }
  };
}
```

__Example__

```tsx
const styles = props => ({
  root: [
    {
      background: props.theme.palette.themePrimary,
      selectors: {
        ':hover': {
          background: props.theme.palette.themeSecondary,
        }
      }
    },
    props.isExpanded
    ? { display: 'block' }
    : { display: 'none' }
  ]
});

```


## Styling best practices

### Put selectors last

While the order of properties generally doesn't matter (alphabetical is a fair default if you have no other preference), the `selectors` property should come last. This improves readability by preventing a single property from 'hiding' below a large `selectors` property.

```js
element: {
  color: 'blue',
  margin: 0,
  overflow: 'inherit',
  padding: 0,
  textOverflow: 'inherit',
  selectors: {
    '&:hover': {
      color: 'red',
      margin: 10
    }
  }
}
```

### font-size-x variables

Use typesafe enums instead of the sass variables:

```ts
import {
  FontSizes,
  FontWeights,
} from 'office-ui-fabric-react/lib/Styling';

return ({
  root: [
    'ms-ComponentName',
    {
      fontSize: FontSizes.small,
      fontWeight: FontWeights.medium,
    }
  ]
})
```

Or, use the fonts from the `theme` so theme overrides work on fonts as well:

```ts
const { theme } = this.props;
const { palette, semanticColors, fonts } = theme;
const font = fonts.large;

return ({
  root: [
    'ms-ComponentName',
    {
      fontFamily: font.fontFamily,
      fontSize: font.fontSize,
      fontWeight: font.fontWeight,
    }
  ]
})
```

### Focus rectangles

The `styling` package has a helper to provide consistent focus rectangles.

```ts
import { getFocusStyle } from 'office-ui-fabric-react';

return ({
  root: [
    getFocusStyle(/* theme, inset, position, highContrastStyle */),
    {
        // Other Styles
    }
  ]
})
```

# Customizing Your App 

So far with Themes and component `styles` props, you have a very general global set of styles, and specific, hyper localized component styles. To make themes less global, and `styles` less localized, we also support a Customizer component. This component lets you do the following:

1. Theme of a specific region of your application (like sidebar, or modal) using `settings`
2. Apply custom component props (including styling) to all components inside the customized area with `scopedSettings`

## Settings

The `settings` prop of Customizer allows you to pass in a full theme to a specific region of your application. In the example below only the second button is affected by these style changes. Check out [this codepen](https://codepen.io/micahgodbolt/pen/mGjxoP?editors=0110) to see this in action.

```jsx

import { createTheme, PrimaryButton, Customizer } from 'office-ui-fabric-react';

// Define custom settings including a customized theme
const CustomSettings = {
  theme: createTheme({
    palette: {
      themePrimary: "green",
      white: "yellow"
    },
    semanticColors: {
      buttonBorder: 'red'
    },
    fonts: {
      medium: {
        fontSize: '20px'
      }
    }
  })
};


// Then in  your app
<PrimaryButton text="Regular Button" />

<Customizer settings={CustomSettings}>
  <PrimaryButton text="Custom Button" />
</Customizer>
```

In this case the button uses `theme.pallete.white` for its text, and `theme.pallete.themePrimary` for its background. Semantic color `buttonBorder` is used for its border, and the text size is set using `theme.fonts.medium`. So while using custom themes is a great way to change values across your entire application, it is more difficult to use if you are trying to target specific components, and has the side effect of changing every component that inherits these theme styles. If you are looking to customize just a handful of components, or just need much more granular control, that's why Customize also provides a `scopedSettings` property.

## Scoped Settings

Scoped settings allow you to apply styles and themes to specific components inside of the Customizer. 

In the example below you can see that we created a simple `purpleTheme` that is passed into both Toggle and Checkbox scoped themes. Then, using each component's `styles` interface we change component styles, and even take advantage of the new, in context theme colors. And just like the `styles` examples above, we can pass in style objects, classNames, or a combination of them. Check out [this codepen](https://codepen.io/micahgodbolt/pen/LJBmJR?editors=0110) to try creating your own scoped settings.

```jsx
let purpleTheme = createTheme({
  palette: {
    themePrimary: 'purple'
  },
  semanticColors: {
    inputBackground: 'purple',
    inputBackgroundChecked: 'purple',
    inputBackgroundCheckedHovered: 'purple'
  }
})

const ScopedSettings = {
  Toggle: {
    theme: purpleTheme,
    styles: (props) => ({
      root: 'custom-class-name',
      pill: {
        borderWidth: 2,
        boxShadow: `2px 2px 2px ${props.checked ? 
          props.theme.palette.themePrimary :
          props.theme.palette.neutralPrimary}`
      }
    })
  },
  Checkbox: {
    theme: purpleTheme,
    styles: {
      label: {fontWeight: 'bold'}
    }
  }
};


// In your App

<Checkbox label="Check Me" />

<Toggle onText="On" offText="Off" />

<PrimaryButton text="Not Purple" />  
```
