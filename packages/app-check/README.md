# @capacitor-firebase/app-check

⚡️ Capacitor plugin for Firebase App Check.

## Installation

```bash
npm install @capacitor-firebase/app-check firebase
npx cap sync
```

Add Firebase to your project if you haven't already ([Android](https://firebase.google.com/docs/android/setup) / [iOS](https://firebase.google.com/docs/ios/setup) / [Web](https://firebase.google.com/docs/web/setup)).

### Android

See [Set up your Firebase project](https://firebase.google.com/docs/app-check/android/play-integrity-provider#project-setup) and follow the instructions to set up your app correctly.

#### Variables

This plugin will use the following project variables (defined in your app’s `variables.gradle` file):

- `$firebaseAppCheckPlayIntegrityVersion` version of `com.google.firebase:firebase-appcheck-playintegrity` (default: `16.1.0`)
- `$firebaseAppCheckDebugVersion` version of `com.google.firebase:firebase-appcheck-debug` (default: `16.1.0`)

### iOS

On **iOS 14 and later**, see [Set up your Firebase project](https://firebase.google.com/docs/app-check/ios/app-attest-provider#project-setup) and follow the instructions to set up your app correctly.

On **iOS 13**, see [Set up your Firebase project](https://firebase.google.com/docs/app-check/ios/devicecheck-provider#project-setup) and follow the instructions to set up your app correctly.

Make sure that the private key (\*.p8) you upload to Firebase has `DeviceCheck` selected as a service.

### Web

See [Set up your Firebase project](https://firebase.google.com/docs/app-check/web/recaptcha-provider#project-setup) and follow the instructions to set up your app correctly.

## Configuration

No configuration required for this plugin.

## Demo

A working example can be found here: [robingenz/capacitor-firebase-plugin-demo](https://github.com/robingenz/capacitor-firebase-plugin-demo)

## Usage

```typescript
import { FirebaseAppCheck } from '@capacitor-firebase/app-check';
import { getApp } from 'firebase/app';
import { initializeAppCheck, CustomProvider } from 'firebase/app-check';

const getToken = async () => {
  const { token } = FirebaseAppCheck.getToken({
    forceRefresh: false,
  });
  return token;
};

const initializeOnWeb = async () => {
  await FirebaseAppCheck.initialize({
    siteKey: 'myKey',
  });
};

// Only necessary if you want to use this plugin
// with Firebase JS SDK on Android and iOS.
const initializeOnNative = async () => {
  await FirebaseAppCheck.initialize();
  const provider = new CustomProvider({
    getToken: () => {
      return FirebaseAppCheck.getToken();
    },
  });
  const app = getApp();
  await initializeAppCheck(app, { provider });
};

const setTokenAutoRefreshEnabled = async () => {
  await FirebaseAppCheck.setTokenAutoRefreshEnabled({ enabled: true });
};

const addTokenChangedListener = async () => {
  await FirebaseAppCheck.addListener('tokenChanged', event => {
    console.log('tokenChanged', { event });
  });
};

const removeAllListeners = async () => {
  await FirebaseAppCheck.removeAllListeners();
};
```

## API

<docgen-index>

* [`getToken(...)`](#gettoken)
* [`initialize(...)`](#initialize)
* [`setTokenAutoRefreshEnabled(...)`](#settokenautorefreshenabled)
* [`addListener('tokenChanged', ...)`](#addlistenertokenchanged)
* [`removeAllListeners()`](#removealllisteners)
* [Interfaces](#interfaces)
* [Type Aliases](#type-aliases)

</docgen-index>

<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### getToken(...)

```typescript
getToken(options?: GetTokenOptions | undefined) => Promise<GetTokenResult>
```

Get the current App Check token.

| Param         | Type                                                        |
| ------------- | ----------------------------------------------------------- |
| **`options`** | <code><a href="#gettokenoptions">GetTokenOptions</a></code> |

**Returns:** <code>Promise&lt;<a href="#gettokenresult">GetTokenResult</a>&gt;</code>

**Since:** 1.3.0

--------------------


### initialize(...)

```typescript
initialize(options?: InitializeOptions | undefined) => Promise<void>
```

Activate App Check for the given app.
Can be called only once per app.

| Param         | Type                                                            |
| ------------- | --------------------------------------------------------------- |
| **`options`** | <code><a href="#initializeoptions">InitializeOptions</a></code> |

**Since:** 1.3.0

--------------------


### setTokenAutoRefreshEnabled(...)

```typescript
setTokenAutoRefreshEnabled(options: SetTokenAutoRefreshEnabledOptions) => Promise<void>
```

Set whether the App Check token should be refreshed automatically or not.

| Param         | Type                                                                                            |
| ------------- | ----------------------------------------------------------------------------------------------- |
| **`options`** | <code><a href="#settokenautorefreshenabledoptions">SetTokenAutoRefreshEnabledOptions</a></code> |

**Since:** 1.3.0

--------------------


### addListener('tokenChanged', ...)

```typescript
addListener(eventName: 'tokenChanged', listenerFunc: TokenChangedListener) => Promise<PluginListenerHandle> & PluginListenerHandle
```

Called when the App Check token changed.

Only available for Web.

| Param              | Type                                                                  |
| ------------------ | --------------------------------------------------------------------- |
| **`eventName`**    | <code>'tokenChanged'</code>                                           |
| **`listenerFunc`** | <code><a href="#tokenchangedlistener">TokenChangedListener</a></code> |

**Returns:** <code>Promise&lt;<a href="#pluginlistenerhandle">PluginListenerHandle</a>&gt; & <a href="#pluginlistenerhandle">PluginListenerHandle</a></code>

**Since:** 1.3.0

--------------------


### removeAllListeners()

```typescript
removeAllListeners() => Promise<void>
```

Remove all listeners for this plugin.

Only available for Web.

**Since:** 1.3.0

--------------------


### Interfaces


#### GetTokenResult

| Prop                   | Type                | Description                                                                                                      | Since |
| ---------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------- | ----- |
| **`token`**            | <code>string</code> | The App Check token in JWT format.                                                                               | 1.3.0 |
| **`expireTimeMillis`** | <code>number</code> | The timestamp after which the token will expire in milliseconds since epoch. Only available for Android and iOS. | 1.3.0 |


#### GetTokenOptions

| Prop               | Type                 | Description                                                                                                 | Default            | Since |
| ------------------ | -------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------ | ----- |
| **`forceRefresh`** | <code>boolean</code> | If `true`, will always try to fetch a fresh token. If `false`, will use a cached token if found in storage. | <code>false</code> | 1.3.0 |


#### InitializeOptions

| Prop                            | Type                 | Description                                                                                                                                                                                                                                                                                                                                     | Default            | Since |
| ------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | ----- |
| **`debug`**                     | <code>boolean</code> | If `true`, the debug provider is used. ⚠️ **Attention**: The debug provider allows access to your Firebase resources from unverified devices. Don't use the debug provider in production builds of your app, and don't share your debug builds with untrusted parties. Read more: https://firebase.google.com/docs/app-check/web/debug-provider | <code>false</code> | 1.3.0 |
| **`isTokenAutoRefreshEnabled`** | <code>boolean</code> | If `true`, the SDK automatically refreshes App Check tokens as needed.                                                                                                                                                                                                                                                                          | <code>false</code> | 1.3.0 |
| **`siteKey`**                   | <code>string</code>  | The reCAPTCHA v3 site key (public key). Only available for Web.                                                                                                                                                                                                                                                                                 |                    | 1.3.0 |


#### InstanceFactoryOptions

| Prop                     | Type                |
| ------------------------ | ------------------- |
| **`instanceIdentifier`** | <code>string</code> |
| **`options`**            | <code>{}</code>     |


#### SetTokenAutoRefreshEnabledOptions

| Prop          | Type                 | Description                                                                                                                      | Since |
| ------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----- |
| **`enabled`** | <code>boolean</code> | If `true`, the SDK automatically refreshes App Check tokens as needed. This overrides any value set during initializeAppCheck(). | 1.3.0 |


#### PluginListenerHandle

| Prop         | Type                                      |
| ------------ | ----------------------------------------- |
| **`remove`** | <code>() =&gt; Promise&lt;void&gt;</code> |


#### TokenChangedEvent

| Prop        | Type                | Description                        | Since |
| ----------- | ------------------- | ---------------------------------- | ----- |
| **`token`** | <code>string</code> | The App Check token in JWT format. | 1.3.0 |


### Type Aliases


#### InitializeOptions

<code><a href="#instancefactoryoptions">InstanceFactoryOptions</a></code>


#### TokenChangedListener

Callback to receive the token change event.

<code>(event: <a href="#tokenchangedevent">TokenChangedEvent</a>): void</code>

</docgen-api>

## Changelog

See [CHANGELOG.md](/packages/app-check/CHANGELOG.md).

## License

See [LICENSE](/packages/app-check/LICENSE).
