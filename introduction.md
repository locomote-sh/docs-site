---
title: Introduction
layout: documentation
---
# Locomote

## What is it?

_Locomote_ is a headless content management system (CMS) used to build and publish websites and progressive web apps (PWAs) with offline capabilities.

A website or PWA is offline capable if it is able to continue operating normally despite the absence of an internet connection. What this means in practice is that the page or app is able to load files and query for data even when no network connection is available.

Offline capabilities can be very useful on mobile devices with intermittent connectivity, but more generally such capabilities can be used to ensure quality of service (QoS) for websites through reduced page latency, improved page load times, and reduced network traffic.

Browser support for fully seamless offline capabilities has only recently been available across all major browsers through the service worker and associated web APIs (see <https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API>, <https://developers.google.com/web/fundamentals/primers/service-workers/>). A service worker is essentially a web proxy that runs within the browser, and allows page requests to be intercepted and handled locally within the browser instead of being sent out over the network. Service workers are a powerful and flexible technology, but require considerable effort and expertise to setup and test.

_Locomote_ makes it trivially easy to incorporate service worker functionality into a website or PWA.

## Yes OK, but what is it?

_Locomote_ is a headless CMS - this means that instead of through a GUI, content is managed via command line tools and/or an API.

_Locomote_ is actually built on [git](https://git-scm.com/), and uses _git_ as a file database for storing content. Because of this, the primary tools used to manage content in _Locomote_ are the git command line tools (or any GUI equivalent) - this means that if you’re a developer then you probably already know how to add, update or delete content in _Locomote_!

Because it is based on _git_, all content in _Locomote_ is stored in repositories - called content repositories. Furthermore, _Locomote_ source and published content on different branches of the same repository, typically using the master branch to store web assets and data, and a branch named public to store built and published content. (If you’ve ever used _GitHub pages_ then you’ll be familiar with the concept). Content on the public branch is then publically accessible and can be loaded in a browser using the content repository’s public URL.

_Locomote_ provides a client SDK which runs in the browser. The client SDK manages the download of content from the server, and manages access to that content from within the browser. The core client SDK provides a lightweight (less than 20k) service worker implementation whose functionality can be extended through plugins.

Not all browsers currently support service workers and the associated set of web APIs. _Locomote_ is fully backward compatible with older browsers, and websites and PWAs built using _Locomote_ will load and run in them, but without offline capability.

_Locomote_ provides a workflow based build system and a suite of build tools for generating published content from assets and data added to a content repository. Builds can be run locally on the developer’s machine, or executed automatically on the server when content updates are pushed to it. The default build tools can be used to do things like generate service worker code or a web manifest for your site, generate icons and splashscreen images for your PWA, or build and style markdown into HTML.

## What next?

* A number of step-by-step tutorials describing how to use _Locomote_ to build and publish content, togeter with some code sample walkthroughs, are available [here]({{ site.baseurl }}/tutorials-and-samples/index.html).
* A detailed description of content repositories and how to work with and publish content is available [here]({{ site.baseurl }}/content-management.html).
* An overview of the client SDK is available [here]({{ site.baseurl }}/client-sdk.html).
* Descriptions of the build tools and build workflow are available [here]({{ site.baseurl }}/content-builds.html).
* Detailed reference document is available [here]({{ site.baseurl }}/reference/index.html)
* Or see the [FAQ]({{ site.baseurl }}/faq.html).
