# unsplash-cache

An nginx config to cache [unsplash](DOCKERHUB_TOKEN), to get around their very annoying rate limit

Unsplash has a rate limit of 50 requests per hour for unverified API clients. Their verification process is rather complex (and the use case of a static-site generator is apparently unheard off), so there needs to be an alternative.

This `nginx` config (and resulting Docker container) caches Unsplash's API, allowing it to act as a centralised cache for API requests, bypassing the API restrictions. A valid API token is still required.

## Usage

```yml
version: "2.3"

services:
  web:
    image: theorangeone/unsplash-cache:latest
    tmpfs:
      - /tmp/unsplash-cache
    ports:
      - "80:80"
```

If you want the cache to persist between container restarts, mount `/tmp/unsplash-cache` to a given directory.

Instead of sending requests to `api.unsplash.com`, send them to this container instead.

## Caching

- Cached responses are kept for 21 days.
- The cache cannot grow beyond 1GB.
- The cache key size should store around 10k records
