# KhulnaSoft Runners Cache action

This utility repository periodically runs a GitHub Action that pulls the latest [actions/cache](https://github.com/actions/cache) repository and applies a rather simple patch to all of its actions:

```ts
const httpCacheHost = process.env["KHULNASOFT_HTTP_CACHE_HOST"];

if (httpCacheHost != null) {
    const newActionsCacheURL = `http://${httpCacheHost}/`;

    console.log(
        `Redefining the ACTIONS_CACHE_URL to ${newActionsCacheURL} to make the cache faster...`
    );

    process.env["ACTIONS_CACHE_URL"] = newActionsCacheURL;
}
```

This allows the tasks running on Khulnasoft Runners to take advantage of a faster and more local cache provided by Khulnasoft Runners and exposed in `KHULNASOFT_HTTP_CACHE_HOST` environment variable.

These modifications are then re-pushed to the corresponding major tags (e.g. `v4`), making the changes in your CI workflows as simple as:

```diff
-- uses: actions/cache@v4
+- uses: khulnasoft/cache@v4
   with:
     path: node_modules
     key: node_modules
```

## Running with your own implementation of GitHub Actions Cache API

If you have implemented the GitHub Actions Cache API alongside self-hosted runners, you can enable its use by setting the `KHULNASOFT_HTTP_CACHE_HOST` environment variable when starting your runner:

```bash
export KHULNASOFT_HTTP_CACHE_HOST=cache.internal:8080
./run.sh
```
