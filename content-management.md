---
title: Content Management
layout: documentation
---

# Content Management

This section describes how to create, build and publish content using _Locomote_.

# Overview

_Locomote_ is a file-based CMS; this means that all content - whether page assets like images, styles and JS files, or data like actual page content - is represented using files. _Locomote_ actually uses _git_ to store and manage content, with the content for a single website or PWA typically stored in a single content repository.

The following diagram gives an overview of the content management lifecycle:

![Content workflow]({{ site.baseurl }}/content-workflow.svg)


1. Content is stored as files within a content repository. The master branch contains all the files that make up the website or PWA source code - e.g. HTML, markdown, image, style and JS files etc.. A local copy of the content repository is cloned in order to make changes to the source.
2. Content is created or modified by editing files within the repository. Once changes have been made, they are added and committed to the repository using _git_, before being pushed back to the server.
3. After the changes are received on the server they are passed to the build workflow, which generates the published version of the content, and the writes the build results to the content repository’s public branch.
4. Any web browser is able to load the published content from the content repository’s public branch via the repository’s public URL. A service worker running within the browser is able to (a) download and cache content from the public branch, and then (b) handle web page requests by serving content from the local cache.
5. When a service worker isn’t available (which will happen before the service worker has installed, or on older browsers with no service worker support) then the web page simply loads content directly from the public branch of the content repository.

# Content repositories

## Branches

By default, files within a content repository are organized into two branches, _master_ and _public_. The _master_ branch is used to store all the source assets and data needed to build the website or PWA. Built content is written to the _public_ branch from where it is publicly accessible.

By default, only content on the _public_ branch is publicly visible to the web via the repository’s public URL.

### master branch
[describe default layout of content on master branch]

### public branch
[describe default layout of content on public branch, including brief discussion of filesets, and which files are explicitly excluded from public access]

## Public URLs

Every content repository has a unique public URL which allows content within that repository to be accessed over the web. Normally, that URL is formed from the repository and _Locomote_ account name, e.g.:

> https://locomote.sh/my-account/my-repository/

The public URL will load content from the public branch of the referenced content repository.

## Locomote manifest

The _Locomote manifest_ is a JSON file, named `locomote.json`, which may optionally be included in the root folder of a content repository’s _master_ branch in order to provide additional configuration around content builds and content publishing.

The different configuration options available through this file are described in detail in the [Locomote manifest]({{ site.baseurl }}/reference/locomote-manifest.html) section of the reference guide.

# Working with content

Because all content is stored as files within a git repository, you can use the same tools to work with _Locomote_ content as you would to work with any other git repository.

The basic steps to publishing content are:

* Create a content repository. You do this through the _Locomote_ [dashboard](https://cms.locomote.sh).
* Once the content repository is created, create a local copy of it on your work machine by cloning the repository from the server. Find and copy the repository’s clone URL in the dashboard, and then using _git_:
```
    git clone <url>
```
* Once the repository is cloned, you can add, edit or delete files using any file editor and the standard git commands for managing content. For example, once files have been added or updated, use _git add_ to add the changes to the git index:
```
    git add -A .
```
* After that, commit the changes and push them to the server:
```
    git commit
    git push
```
* The build workflow will be invoked once on the server, and the changes will become publicly visible once built and added to the repository’s public branch.

# Content builds

The content build process is responsible for generating publishable content from the source content on a content repository’s _master_ branch. The default build process does this by applying a number of build operations to the source content; these build operations are:

* **Image resizing**: This operation generates app icons and splashscreen images for a PWA from one or more source images.
* **Service worker generation**: This operation generates a custom service worker for the website or PWA, using settings provided in the Locomote manifest.
* **Web app manifest**: This operation generates a web app manifest file (manifest.webmanifest) for a PWA from settings in the Locomote manifest.
* **PWA HTML header**: This operation generates a &lt;head&gt; section for a HTML page from settings in the Locomote manifest; this is needed for full PWA support on iOS, and includes &lt;meta&gt; and &lt;link&gt; tags to configure app icons, splashscreens and colours on iOS. This operation also generates an app banner for iOS which prompts the user to save the PWA to their desktop (similar to the default banner on Android).

The content build process is described in more detail in the [content build section]({{ site.baseurl }}/content-builds.html) of this guide.

Individual build operations are described in more detail in the [reference guide]({{ site.baseurl }}/reference/build-operations.html).
