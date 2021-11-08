---
layout: standard
toc: false
---

:::warning

This page is an archive for v1.x and v2.x of Fable.Elmish.HMR package

:::

## Hot Module Replacement

Elmish applications can benefit from Hot Module Replacement (known as HMR).

This allow us to modify the application while it's running, without a full reload. Your application will now maintain its state between two changes.

![hmr demo](/hmr/img/hmr_demo.gif)


## Installation
Add Fable package with paket:

```sh
paket add nuget Fable.Elmish.HMR
```

## Webpack configuration

Add `hot: true` and `inline: true` to your `devServer` node.

Example:

```js
devServer: {
    contentBase: resolve('./public'),
    port: 8080,
    hot: true,
    inline: true
}
```

You also need to add this two plugins when building in development mode:

- `webpack.HotModuleReplacementPlugin`
- `webpack.NamedModulesPlugin`

Example:

```js
plugins : isProduction ? [] : [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NamedModulesPlugin()
]
```

You can find a complete `webpack.config.js` [here](https://github.com/elmish/templates/blob/master/src/react-demo/Content/webpack.config.js).

## Program module functions

Augment your program instance with HMR support.

*IMPORTANT*: Make sure to add HMR support before `Program.withReact` or `Program.withReactNative` line.

Usage:

```fs
open Elmish.HMR

Program.mkProgram init update view
|> Program.withHMR // Add the HMR support
|> Program.withReact "elmish-app"
|> Program.run
```

and if you use React Native:


```fs
open Elmish.HMR

Program.mkProgram init update view
|> Program.withHMR // Add the HMR support
|> Program.withReactNative "elmish-app"
|> Program.run
```

## Conditional compilation

If don't want to include the Hot Moduple Replacement in production builds surround it with `#if DEBUG`/`#endif` and define the symbol conditionally in your build system.
