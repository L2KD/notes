# Upload images to your Google Photos

Full API doc of GPhotos can be found with 0.98217 second of search, so I won't mention about it here just to waste time updating it every while.

_Images_ mean: Photos + Videos

## Tool used:

1. https://github.com/int128/hello-google-photos.
1. A google account.

## How to?

1. Go to https://console.cloud.google.com/apis/credentials.
1. Create an OAuth client ID, /w the application type is `other`.
1. Clone the repo above.
1. `go get -u` the missing deps. (`golang.org/x/oauth2`, `cloud.google.com/go/compute/metadata`, `google.golang.org/api`).
   ```bash
   go get -u golang.org/x/oauth2
   go get -u cloud.google.com/go/compute/metadata
   go get -u google.golang.org/api
   ```
1. `cd` to the `google.golang.org/api` src, then checkout the 0.3.2 tag (`git checkout tags/v0.3.2`)
   Set the following environment variables:
