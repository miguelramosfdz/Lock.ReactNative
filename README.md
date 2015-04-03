# LockReact

[![CI Status](http://img.shields.io/travis/auth0/Lock.ReactNative.svg?style=flat)](https://travis-ci.org/auth0/Lock.ReactNative)
[![Version](https://img.shields.io/cocoapods/v/LockReact.svg?style=flat)](http://cocoapods.org/pods/LockReact)
[![License](https://img.shields.io/cocoapods/l/LockReact.svg?style=flat)](http://cocoapods.org/pods/LockReact)
[![Platform](https://img.shields.io/cocoapods/p/LockReact.svg?style=flat)](http://cocoapods.org/pods/LockReact)

[Auth0](https://auth0.com) is an authentication broker that supports social identity providers as well as enterprise identity providers such as Active Directory, LDAP, Google Apps and Salesforce.

**LockReact** is a wrapper around [Lock](https://github.com/auth0/Lock.iOS-OSX) so it easier to use it from React Native.

> LockReact API is in Beta and might be subject to changes due to improvements in either Lock or React Native

## Requirements

iOS 7+ & React Native

## Installation

LockReact is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "LockReact", '~> 0.1'
```

Then in your project's `Info.plist` file add the following entries:

* __Auth0ClientId__: The client ID of your application in _Auth0_. You can find this value in your app's settings in [Auth0 dashboard](https://app.auth0.com/#/applications).
* __Auth0Tenant__: The name of your account, if your account's domain in Auth0 is `myaccount.auth0.com`, your tenant name is `myaccount`.  

For example:

[![Auth0 plist](https://cloudup.com/cdHr2oMAN7d+)](http://auth0.com)

Also you need to register a Custom URL type, it must have a custom scheme with the following format `a0<Your Client ID>`. For example if your Client ID is `Exe6ccNagokLH7mBmzFejP` then the custom scheme should be `a0Exe6ccNagokLH7mBmzFejP`.

Then you'll need to handle that custom scheme, so first import __Lock__ header in your `AppDelegate.m`

```objc
#import <Lock/Lock.h>
```

and override `-application:openURL:sourceApplication:annotation:` method, if you haven't done it before, and add the following line:

```objc
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
  return [[A0IdentityProviderAuthenticator sharedInstance] handleURL:url sourceApplication:sourceApplication];
}
```

> This is required to be able to return back to your application when authenticating with Safari (or native integration with FB or Twitter if used). This call checks the URL and handles all that have the custom scheme defined before.

Then copy [LockReactModule.h](https://raw.githubusercontent.com/auth0/Lock.ReactNative/master/Example/iOS/Modules/LockReactModule.h) and [LockReactModule.m](https://raw.githubusercontent.com/auth0/Lock.ReactNative/master/Example/iOS/Modules/LockReactModule.m) to your Xcode project, and make sure they added to your app's target.

Finally in the file `index.ios.js`, require Lock's module like this:

```js
var Lock = require('NativeModules').LockReactModule;
```

## Usage

### Email/Password, Enterprise & Social authentication

```js
Lock.show({}, (err, profile, token) => {
  console.log('Logged in!');
});
```

And you'll see our native login screen

[![Lock.png](http://blog.auth0.com.s3.amazonaws.com/Lock-Widget-Screenshot.png)](https://auth0.com)

> By default all social authentication will be done using Safari, if you want native integration please check this [wiki page](https://github.com/auth0/Lock.iOS-OSX/wiki/Native-Social-Authentication).

### TouchID

```js
Lock.show({
  connections: ['touchid'],
  authParams: {
    connection: 'Username-Password-Authentication',  
  }
}, (err, profile, token) => {
  console.log('Logged in!');
});
```

And you'll see TouchID login screen

[![Lock.png](http://blog.auth0.com.s3.amazonaws.com/Lock-TouchID-Screenshot.png)](https://auth0.com)

> Because it uses a Database connection, the user can change it's password and authenticate using email/password whenever needed. For example when you change your device.

## API

### Lock

####.show(options, callback)
Show Lock's authentication screen as a modal screen using the connections configured for your applications or the ones specified in the `options` parameter. This is the list of valid options:

* **closable** (`boolean`): If Lock screen can be dismissed
* **connections** (`[string]`): List of enabled connections to use for authentication. Must be enabled in your app's dashboard first.
* **authParams** (`object`): Object with the parameters to be sent to the Authentication API, e.g. `scope`.

The callback will have the error if anything went wrong or after a successful authentication, it will yield the user's profile info and tokens.

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/whitehat) details the procedure for disclosing security issues.

## What is Auth0?

Auth0 helps you to:

* Add authentication with [multiple authentication sources](https://docs.auth0.com/identityproviders), either social like **Google, Facebook, Microsoft Account, LinkedIn, GitHub, Twitter, Box, Salesforce, amont others**, or enterprise identity systems like **Windows Azure AD, Google Apps, Active Directory, ADFS or any SAML Identity Provider**.
* Add authentication through more traditional **[username/password databases](https://docs.auth0.com/mysql-connection-tutorial)**.
* Add support for **[linking different user accounts](https://docs.auth0.com/link-accounts)** with the same user.
* Support for generating signed [Json Web Tokens](https://docs.auth0.com/jwt) to call your APIs and **flow the user identity** securely.
* Analytics of how, when and where users are logging in.
* Pull data from other sources and add it to the user profile, through [JavaScript rules](https://docs.auth0.com/rules).

## Create a free account in Auth0

1. Go to [Auth0](https://auth0.com) and click Sign Up.
2. Use Google, GitHub or Microsoft Account to login.

## Author

Auth0

## License

LockReact is available under the MIT license. See the [LICENSE file](LICENSE) for more info.
