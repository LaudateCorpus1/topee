<img align="right" src="logo.png" width="300px"/>

Are you a JavaScript developer?
-------------------------------

Write Swiftless Safari Extensions using Chrome API
==================================================

Topee provides Chrome Extension API. You can have the same codebase for all browsers.
-------------------------------------------------------------------------------------

While Google Chrome, Firefox and Edge share the same extension API, Safari extensions are going in a different direction,
requiring developers to use a different set of APIs and partly also Swift instead of JavaScript.

The Topee project provides the Google Chrome Extension API for Safari, allowing you to run a single codebase on all the browsers.

The API is [complete enough](http://htmlpreview.github.io/?https://github.com/avast/topee/blob/master/api.html#) to run in production, such as the extension delivered with [Avast Passwords for Mac](https://www.avast.com/passwords#mac).

> Use [carthage](https://github.com/Carthage/Carthage) to [add it to your project](#xcode-project).


First run
=========

If you are completely new to Xcode, read [how to run the Demo application](documentation/xcode_bootstrap.md) first. Also, before you run the extension for the first time, make sure to build the containing application.

Xcode Project
-------------

Detailed setup instruction with pictures are available at [wizard.md](documentation/wizard.md).

- Add `github "https://github.com/avast/topee" ~> 1.0.0` to your Cartfile
- Link the `Topee` to your Appex target
- Add a resource to your Appex pointing to `Carthage/Build/Mac/Topee.framework/Resources/topee-content.js`
- Make sure the `topee-content.js` script is referenced in the plist of your appex under: `NSExtension / SFSafariContentScript` and appears as the first script in the array
- Add a subclass of `TopeeSafariExtensionHandler` and reference your background script in the `NSExtension / TopeeSafariBackgroundScript` attribute like shown in the snippet below
- Make sure the the plist entry: `NSExtension > NSExtensionPrincipalClass` of your appex plist points to `$(PRODUCT_MODULE_NAME).SafariExtensionHandler` where `SafariExtensionHandler` is the name of your `TopeeSafariExtensionHandler` subclass

See [xcode.md](xcode.md) for more details.

Extension parts
---------------

Content scripts are listed in Info.plist as [specified](https://developer.apple.com/documentation/safariservices/safari_app_extensions/injecting_a_script_into_a_webpage) for the app extensions.
Make sure to list Topee API (topee-content.js) as the first one.
Content scripts are injected when document starts loading by default. `src/Framework/Scripts/run-at-document-end.sh` preprocesses them at build time to run when document body already exists.

Background scripts are also listed in Info.plist in the same manner as content scripts, under SFSafariBackgroundScript key.
topee-background.js should not be listed, but place it into the Appex resources.

Injected iframes from web accessible resources need to reference topee-iframe-resources.js.

Place browserAction icons into the Appex resources.

How it works
====

![documentation/integration.svg](https://raw.github.com/avast/topee/master/documentation/integration.svg?sanitize=true)
To run Chrome-API-based extension, Topee creates an invisible WebView to run background scripts
and `chrome` namespace implementations for background and content scripts, that provides [messaging](messaging.md)
and other functionality.

> Use [carthage](https://github.com/Carthage/Carthage) to [add it to your project](#xcode-project).


Tests
====

Once you have the demo extension installed in Safari, you can run unit tests by visiting https://pamcdn.avast.com/pamcdn/extensions/install/mac/blank.html
