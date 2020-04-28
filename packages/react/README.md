# Matomo Tracker (React)

Stand alone library for using Matamo tracking in React projects

## Installation

Installing with npm: `npm i --save @datapunt/matomo-tracker-react` or yarn `yarn add @datapunt/matomo-tracker-react`

## Usage

Before you're able to use this Matomo Tracker you need to create a Matomo instance with your project specific details, and wrap your application with the `MatomoProvider` that this package exposes.

```js
import { MatomoProvider, createInstance } from '@datapunt/matomo-tracker-react'

const instance = createInstance({
  urlBase: "https://LINK.TO.DOMAIN",
  siteId: 3, // optional, default value: `1`
  trackerUrl: "https://LINK.TO.DOMAIN/tracking.php", // optional, default value: `${urlBase}matomo.php`
  srcUrl: "https://LINK.TO.DOMAIN/tracking.js" // optional, default value: `${urlBase}matomo.js`
});

ReactDOM.render(
  <MatomoProvider value={instance}>
    <MyApp />
  </MatomoProvider>
)
```

After wrapping your application with the `MatomoProvider` you can use the `useMatomo` hook to track your application from anywhere within the MatomoProvider component tree:

```js
import React from 'react'
import { useMatomo } from '@datapunt/matomo-tracker-react'

const MyPage = () => {
  const { trackPageView, trackEvent } = useMatomo()

  // Track page view
  React.useEffect(() => {
    trackPageView()
  }, [])

  const handleOnClick = () => {
    // Track click on button
    trackEvent({ category: 'sample-page', action: 'click-event' })
  }

  return <button type="button" onClick={handleOnClick}>Click me</button>
}
```

## Advanced usage

By default the Matomo Tracker will send the window's document title and location, or send your own values. Also, [custom dimensions](https://matomo.org/docs/custom-dimensions/) can be used:

```js
import React from 'react'
import { useMatomo } from '@datapunt/matomo-tracker-react'

const MyPage = () => {
  const { trackPageView, trackEvent } = useMatomo()

  // Track page view
  React.useEffect(() => {
    trackPageView({
      documentTitle: 'Page title', // optional
      href: 'https://LINK.TO.PAGE', // optional
      customDimensions: [
        {
          id: 1,
          value: 'loggedIn',
        },
      ], // optional
    })
  }, [])

  const handleOnClick = () => {
    // Track click on button
    trackEvent({ category: 'sample-page', action: 'click-event' })
  }

  return <button type="button" onClick={handleOnClick}>Click me</button>
}
```

And you can do the same for the `trackEvent` method:

```js
import React from 'react'
import { useMatomo } from '@datapunt/matomo-tracker-react'

const MyPage = () => {
  const { trackEvent } = useMatomo()

  const handleOnClick = () => {
    // Track click on button
    trackEvent({
      category: 'sample-page',
      action: 'click-event',
      name: 'test', // optional
      value: 123, // optional, numerical value
      documentTitle: 'Page title', // optional
      href: 'https://LINK.TO.PAGE', // optional
      customDimensions: [
        {
          id: 1,
          value: 'loggedIn',
        },
      ], // optional
    })
  }

  return <button type="button" onClick={handleOnClick}>Click me</button>
}
```

## References

- [Matomo JavaScript Tracking Guide](https://developer.matomo.org/guides/tracking-javascript-guide)
