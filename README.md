# Fathom Client [![CircleCI](https://circleci.com/gh/unstacked/fathom-client.svg?style=svg)](https://circleci.com/gh/unstacked/fathom-client)

A [Fathom Analytics](https://usefathom.com/) library for environments with client-side routing.

Extracted from the [StaticKit](https://statickit.com) website.

## Installation

Run the following to install in your project:

```
npm install fathom-client
```

## Motivation

The standard installation flow for Fathom is to drop their snippet on your page, which will automatically load the library and track a pageview. This approach works great for server-rendered sites with full page refreshes, but gets tricky when:

- Routing happens on the client-side (e.g. an SPA)
- The DOM is abstracted away (e.g. Next.js)

This library provides an interface you can use to orchestrate Fathom calls at various points in your page lifecycle:

```js
import * as Fathom from 'fathom-client';

// Upon initial page load...
Fathom.load();
Fathom.setSiteId('XXXXXXXX');
Fathom.trackPageview();

// In the route changed event handler...
const onRouteChangeComplete = () => {
  Fathom.trackPageview();
};

// In an event handler where a goal is achieved...
const onSignUp = () => {
  Fathom.trackGoal('Sign Up', 100);
};
```

## Usage

### Next.js

Create an `_app.js` file in your `pages` directory, [like this](https://nextjs.org/docs#custom-app):

```jsx
import React, { useEffect } from 'react';
import Router from 'next/router';
import * as Fathom from 'fathom-client';

// Record a pageview when route changes
Router.events.on('routeChangeComplete', () => {
  Fathom.trackPageview();
});

function App({ Component, pageProps }) {
  // Initialize Fathom when the app loads
  useEffect(() => {
    if (process.env.NODE_ENV === 'production') {
      Fathom.load();
      Fathom.setSiteId('ZFEWBXJZ');
      Fathom.trackPageview();
    }
  }, []);

  return <Component {...pageProps} />;
}

export default App;
```

## Releasing

Run the following to publish a new version:

```bash
npm run release
```
