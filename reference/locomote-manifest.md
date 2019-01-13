---
title: Locomote Manifest
layout: documentation
---
# Locomote Manifest File

# Overview

The Locomote manifest file is used to control how content is built and published by _Locomote_. The manifest is a JSON file named locomote.json and placed in the root folder of the master branch of a content repository. The manifest supports several different categories of setting with properties in each category grouped under a common property name; the following sections describe each category and give an overview of the settings available in each category.

# Service worker

The Locomote SDK provides a standard service worker component which is used to provide offline support to pages and progressive web apps (PWAs) loaded from a Locomote content repository. Manifest options under the top-level serviceWorker property can be used to customize and extend the default behaviour of this service worker.

## Content origins

Normally, the service worker loads and caches data only from the content repository that it is loaded from, but data can also be loaded from additional content repos by listing their public URLs in the manifest.

## Caching external resources

Not all resources loaded by a page are stored within the page’s content repo - for example, page assets like font files may also be loaded from a CDN. Listing the URLs of such resources in the manifest allows the service worker to ensure their availability when offline by pre-caching those assets.

## Plugins

Additional functionality - such as full text search - can be added to the service worker by listing plugin names or URLs in the manifest.

## Version

When building the service worker for a content repository, _Locomote_ will by default use the latest version of the standard Locomote service worker code but can be locked to a specific version of the code by specifying a version number in the manifest.

# Progressive web apps

Options for configuring and publishing PWAs appear under the `"pwa"` property in the manifest. Many of these options correspond with standard options that appear in the app’s web manifest file - _Locomote_ will automatically generate a web manifest (as well as &lt;link&gt; and &lt;meta&gt; HTML elements in the page for full iOS compatibility) from the values in the Locomote manifest. It is also possible to provide a `manifest.webmanifest` file in the content repo - settings in the manifest will be merged into the web manifest, with settings in the provided file taking precedence over default settings and settings defined in the Locomote manifest.

## Installation banner

When displaying a PWA in the browser, Android systems will automatically display a banner prompting the user to save the app to their device’s home screen. iOS devices don’t provide any equivalent behaviour, but _Locomote_ will automatically generate a banner for display on iOS devices. If this behaviour isn’t desired then it can be disabled within the Locomote manifest.

# Manifest File Properties

The following is a list of the properties that can be used within a Locomote manifest file:


| **Name** | **Description** |
|----------|-----------------|
| `serviceWorker` | Service worker configuration options. |
| `  version` | The version of the Locomote service worker to use; defaults to the current available version. |
| `  origins` | A list of additional content origins. Specified as a content repository public URL. |
| `  plugins` | A list of service worker plugin names or script URLs. |
| `  cache` | Additional cache settings. |
| `    static` | A list of static resource URLs which should be cached when the service worker first installs. Use to ensure that resources external of a content repository (e.g. scripts or CSS loaded from a non-Locomote CDN) are available when offline. |
| `pwa` | PWA configuration options. |
| `  name` | The app name (web manifest). |
| `  short_name` | The app short name (web manifest). |
| `  description` | The app description (web manifest). |
| `  display` | The app display option (web manifest). |
| `  background_color` | The app display background colour (web manifest). |
| `  start_url` | The app start URL (web manifest). |
| `  ios` | iOS specific configuration options. |
| `    showInstallBanner` | A flag indicating whether a banner prompting users to install the app to their device home screen should be shown on iOS devices; defaults to `true`. |

# Sample manifest

The following JSON shows a sample Locomote manifest file using the options listed above.

```json
{
  "serviceWorker": {
    "version": "1.0",
    "origins": [
        "https://locomote.sh/samples/data"
    ],
    "plugins": [
        "search",
        "https://example.com/locomote/map-plugin.js"
    ],
    "cache": {
        "addAll": [
"https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css",    "https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
        ]
     }
  },
  "pwa": {
    "name": "Sample App",
    "short_name": "Sample",
    "description": "An app demonstrating some of Locomote.sh’s abilities",
    "display": "fullscreen",
    "background_color": "#FF0000",
    "start_url": "index.html"
   }
}
```
