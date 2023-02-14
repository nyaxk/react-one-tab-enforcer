# Welcome to **[@nyaxk/react-one-tab-enforcer](https://github.com/nyaxk/react-one-tab-enforcer)**!

## This package can used with Typescript and Javascript

## Why
Sometimes your application should make it difficult for users to open your app in multiple tabs. With two tabs you could get your app in one state in one tab, which would not be reflected in the other one. Then making some actions in the first one could result in corrupted state on a backend. Obviously, you should prevent any data corruption in your backend, but if your application requires this extra safety-check, go ahead and use it. :-)

What this package do is - it marks the first tab with the application as the "valid" one for 15 seconds.
Then every 10 seconds it updates this information for another 15 seconds.

If you cleanly close the browser/tab, it clears that information, so you can reopen the app in the new tab right away.
If the close is not clean (think: desktop with a sudden power loss), after 15 seconds you are good again. 

If your app is opened in a different tab within those 15 seconds, it will display a different component with an error message. 

## How

### Install
``` 
npm install --save @nyaxk/react-one-tab-enforcer
```

### Wrap your main component

1. Change
```javascript
export default App
```

to
```javascript
import { withOneTabEnforcer } from "@nyaxk/react-one-tab-enforcer"
(..)
export default withOneTabEnforcer()(App)
```

2. Profit! ;-)

This will work, and display a default "Sorry! You can only have this application opened in one tab" message in place of the App component.

**!IMPORTANT!** 
To make sure we won't collide with other apps that use the same package, we should set a unique app name as an option:

```javascript
export default withOneTabEnforcer({appName: "my-unique-app-name"})(App)
```

## Configuration

Those are the arguments (and their defaults) you can pass as a second argument to the withOneTabEnforcer
```javascript
appName = "default-app-name", // This one you know already - has to be unique!  

OnlyOneTabComponent = DefaultOnlyOneTabComponent, // Component showed in place of the requested one.
localStorageTimeout = 15 * 1000, // (15 seconds) In case that the component will not succeeded clearing the localStorage on closing (desktop PC and a sudden power loss), this is the maximum time your user will have to wait to open your app again in the same browser on the same computer.
localStorageResetInterval = 10 * 1000, // (10 seconds) this is how often the above timeout is reset 
```

For example, if you want to use a custom component that shows up when the user tries to open your app in a new tab, do:

```javascript
const DifferentWarningComponent = () => <div>NO WAY!</div>
export default withOneTabEnforcer({appName: "my-unique-app-name", OnlyOneTabComponent: DifferentWarningComponent})(App)
```


## Credit
This package is closely based on a code from this [jsfiddle](https://jsfiddle.net/yex8k2ts/30/) by [timkellypa](https://stackoverflow.com/users/1257546/timkellypa) .
 and [this jquery demo](https://www.jqueryscript.net/demo/Prevent-Webpage-Opened-Multiple-Tabs-duplicateWindow/). It was packaged for seemless react usage with a bit of a tweaking and made configurable. Unfortunatelly, the code from timkellypa didn't work correctly with chrome "duplicate" functionality.
Credits to https://www.npmjs.com/package/react-one-tab-enforcer