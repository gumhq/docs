---
description: >-
  Using the components in React or Next.js requires an additional Provider
  setup.
---

# ⚙ Setup

For the components to work correctly, you need to set up the GumUIProvider at the root of your application. You can import the provider from `@gumhq/ui-components`.

```tsx
import { GumUIProvider } from '@gumhq/ui-components';
```

## **React**

Go to the root of your application and do this:

```jsx
import * as React from 'react';

// 1. import `GumUIProvider` component
import { GumUIProvider } from '@gumhq/ui-components';

function App({ Component }) {
  // 2. Use at the root of your app
  return (
    <GumUIProvider>
      <Component />
    </GumUIProvider>
  );
}
```

## **Next.js**

1. Go to `pages/_app.js` or `pages/_app.tsx` (create it if it doesn't exist) and add this:

<pre class="language-jsx"><code class="lang-jsx">// 1. import `GumUIProvider` component
import { <a data-footnote-ref href="#user-content-fn-1">GumUIProvider</a> } from '@gumhq/ui-components';

function MyApp({ Component, pageProps }) {
  return (
    // 2. Use at the root of your app
    &#x3C;GumUIProvider>
      &#x3C;Component {...pageProps} />
    &#x3C;/GumUIProvider>
  );
}

export default MyApp;
</code></pre>

2. Go to `pages/_document.js` or `pages/_document.tsx` (create if it doesn't exist) and add this:

```jsx
import React from 'react';
import Document, { Html, Head, Main, NextScript } from 'next/document';
import { CssBaseline } from '@nextui-org/react';

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx);
    return {
      ...initialProps,
      styles: React.Children.toArray([initialProps.styles])
    };
  }

  render() {
    return (
      <Html lang="en">
        <Head>{CssBaseline.flush()}</Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```



## **For React Native**

While no setup is required for using the UI Components library on React Native. You'll most likely find yourself trying to pair it with the library **@solana/web3.js** which does not support react native usage without polyfills installed.

#### Install dependencies

Next, we install the dependencies. The Solana JavaScript SDK, a package to patch the React Native build system (Metro), a secure random number generator, and a fix to patch React Native's missing `URL` class.

```shell
yarn add \
  react-native-get-random-values \
  react-native-url-polyfill
```

#### Patch Babel to use the Hermes transforms

As of August 2022 the template from which new React Native apps are made enables the Hermes JavaScript engine by default but not the Hermes code transforms. Enable them by making the following change to `babel.config.js`:

```diff
  module.exports = {
-   presets: ['module:metro-react-native-babel-preset'],
+   presets: [
+     [
+       'module:metro-react-native-babel-preset',
+       {unstable_transformProfile: 'hermes-stable'},
+     ],
+   ],
};
```

#### Update `index.js`

To load the polyfills, we open the file `index.js` in the root of the project and add the following two lines to the top of the file:

```javascript
import 'react-native-get-random-values';
import 'react-native-url-polyfill/auto';
```

[^1]: 
