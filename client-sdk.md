---
title: Client SDK
layout: documentation
---
# Client SDK

The client SDK is composed of a number of JS libraries designed to run in the web browser, and which provide many of _Locomote’s_ key capabilities.

## Service worker
The _Locomote_ service worker is the core part of the client SDK. It primary responsibility is to synchronize the web browser’s local caches with content on the server, and to handle page requests from the client by serving data from the local caches. The service worker also provides the query API (see below) which provides a flexible content querying mechanism; and the service worker’s core functionality can be extended using plugins.

# Plugins
Plugins allow the service worker’s core functionality to be extended or modified. For example, plugins can provide local full-text search of website content, or implement alternative methods for downloading content updates from the server.

# Query API

The query API is a HTTP based API providing a standardized interface for querying content and data stored in a content repository.

The query API will work in a browser whether or not a service worker for the page is installed and operational. If the service worker is running then all query API requests will be handled locally - meaning that they will respond quickly, and will continue to work when offline. If the service worker is not running then the query request will be sent to and processed on the server (and so won’t work in offline mode).

# Client API

The client API is a JS API designed to be used from within a page, and which provides utility methods for working with service workers. The client API includes functions for performing the following functions:
* installing service workers;
* listing installed service workers;
* sending messages to service workers;
