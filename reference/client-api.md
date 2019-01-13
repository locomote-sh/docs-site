---
title: Client API
layout: documentation
---
# Client API

The client API is a JS API designed to be used from within a web page in order to interact with service workers managing the page. The API is obviously only useful when a service worker is available to interact with. In cases where the service worker isn’t available then most API calls will fail silently.

The client page API is automatically included in each page when that page is built using Locomote’s standard page build operations.

The API is hosted under `window.locomote.sw` and offers the following functions:

### isInstalled()
Tests whether the Locomote service worker is installed. Returns `true` if the service worker is installed.

### refresh( origin )
Refresh locally cached content by downloading updates from the server.

Arguments:
* `origin`: The content origin to refresh, specified as content repository's public URL (e.g. `"https://locomote.sh/example/docs"`). If not provided then a content origin is derived from the URL of the page.

### scheduleRefresh( interval, origin )
Schedule an automatic repeated content refresh.

Arguments:
* `interval`: A refresh interval, specified in minutes.
* `origin`: A content origin URL (see refresh function).

### post( message )
Post a message to the service worker.
