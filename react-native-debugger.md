## React Native Debugger
https://github.com/jhen0409/react-native-debugger

#### React Native Debugger Open
https://github.com/jhen0409/react-native-debugger/tree/master/npm-package

#### Sử dụng React DevTools
* Đối với Wi-Fi:
  - `>= 0.43` - Thay đổi `host` tại `node_modules/react-native/Libraries/Core/Devtools/setupDevtools.js#L28-L30`
```js
   // PlatformConstants.ServerHost.split(':')[0] :
   '192.168.137.1' : // Địa chỉ IP của PC
```

#### React Inspector gặp thông báo "Connecting to React…" với RN ^0.43
```js
const AppState = require('AppState');
const {PlatformConstants} = require('NativeModules');
// const {connectToDevTools} = require('react-devtools-core');
const bakRIC = window.requestIdleCallback;
const bakCIC = window.cancelIdleCallback;

// To ensure react-devtools-core use polyfill instead of requestIdleCallback
window.requestIdleCallback = null;
window.cancelIdleCallback = null;

const {connectToDevTools} = require('react-devtools-core'); // Line 17
window.requestIdleCallback = bakRIC;
window.cancelIdleCallback = bakCIC;
```

#### `/node_modules/react-native/Libraries/Core/Devtools/setupDevtools.js`
```js
'use strict';

if (__DEV__) {
  const AppState = require('AppState');
  const {PlatformConstants} = require('NativeModules');
  // const {connectToDevTools} = require('react-devtools-core');
  const bakRIC = window.requestIdleCallback;
  const bakCIC = window.cancelIdleCallback;

  // To ensure react-devtools-core use polyfill instead of requestIdleCallback
  window.requestIdleCallback = null;
  window.cancelIdleCallback = null;

  const {connectToDevTools} = require('react-devtools-core'); // Line 17
  window.requestIdleCallback = bakRIC;
  window.cancelIdleCallback = bakCIC;

  connectToDevTools({
    isAppActive() {
      // Don't steal the DevTools from currently active app.
      // Note: if you add any AppState subscriptions to this file,
      // you will also need to guard against `AppState.isAvailable`,
      // or the code will throw for bundles that don't have it.
      return AppState.currentState !== 'background';
    },
    // Special case: Genymotion is running on a different host.
    host: PlatformConstants && PlatformConstants.ServerHost ?
      // PlatformConstants.ServerHost.split(':')[0] :
      '192.168.137.1' :
      'localhost',
    // Read the optional global variable for backward compatibility.
    // It was added in https://github.com/facebook/react-native/commit/bf2b435322e89d0aeee8792b1c6e04656c2719a0.
    port: window.__REACT_DEVTOOLS_PORT__,
    resolveRNStyle: require('flattenStyle'),
  });
}
```
