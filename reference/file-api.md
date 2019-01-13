---
title: File API
layout: documentation
---
# File API

The file API is a web API that allows file content and metadata to be loaded over HTTP.

The content of any publicly accessible file within a content repository (e.g. any file on the public branch) can be accessed using a URL in `https://{origin}/{path}` format,  where `{origin}` is the content repository’s public URL and `{path}` is the path to the file relative to the content repository root.

Every file also has a file record associated with it which is a JSON document containing metadata about the file, and which can be accessed by appending `?format=record` to the file path.

File requests will be handled by the Locomote service worker when installed and running. When available, the service worker will handle all file record requests locally, and will only request file content over the network when it is not available locally (i.e. because it is not cached). Otherwise, when the service worker isn’t available, then all file API requests are sent to and handled by the server.

## Example

The URL <https://locomote.sh/example/docs/assets/img/logo.jpg> references an image in one of the example repositories. Here, the content origin is `https://locomote.sh/example/docs` and the file path is `/assets/img/logo.png`. The file record for this image can be accessed by appending `?format=record` to the URL:

<https://locomote.sh/example/docs/assets/img/logo.jpg?format=record>

```json
{
    "path": "assets/img/logo.jpg",
    "category": "images",
    "status": "published",
    "commit": "0db84b6"
}
```
