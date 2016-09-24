# Ultimate developer cheatsheet

----

## :sparkles: The Security Checklist :sparkles:

### Security
- [ ] **Use HTTPS everywhere.**
- [ ] **Patch LOGJAM Vulnerability.**
- [ ] **[Set HTTP Security Headers](HTTP Security Headers)


#### HTTP Security Headers

- [ ] **Content Security Policy (CSP) header.**

    ```Content-Security-Policy "default-src https: data: 'unsafe-inline' 'unsafe-eval'" always;``` :sparkles:

    *Examples*

    > Disable unsafe inline/eval, only allow loading of resources (images, fonts, scripts, etc.) over https (recommended)

    > ```Content-Security-Policy: default-src https:```

    > Disable the use of unsafe inline/eval, allow everything else

    > ```Content-Security-Policy: *```

    > Disable unsafe inline/eval, only load resources from same origin, except also allow images on imgur

    > ```Content-Security-Policy: default-src 'self'; img-src 'self' https://i.imgur.com```

    > Disable unsafe inline/eval, only load resources from same origin, fonts from google, images from same origin and imgur

    > ```Content-Security-Policy: default-src 'self'; font-src 'https://fonts.googleapis.com'; img-src 'self' https://i.imgur.com```

    > Pre-existing site uses too much inline code to fix, but wants to ensure resources are loaded only over https

    > ```Content-Security-Policy: default-src https: 'unsafe-eval' 'unsafe-inline'```


- [ ] **HTTP Strict Transport Security (HSTS) header**

    ```add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"```

- [ ] **X-Content-Type-Options header.** This tells not to load scripts and stylesheets unless the server indicates the correct MIME type
- [ ] **X-Frame-Options header.** This is an HTTP header that allows sites control over how your site may be framed within an iframe.
- [ ] **X-XSS-Protection header.** Stops pages from loading when they detect reflected cross-site scripting (XSS) attacks.

    ``` # Block pages from loading when they detect reflected XSS attacks: X-XSS-Protection: 1; mode=block ```




----
### HTTP Cache Headers
- [ ] **Cache-Control**
    The Cache-Control general-header field is used to specify directives that MUST be obeyed by all the caching system. The syntax is as follows:
    An HTTP client or server can use the Cache-control general header to specify parameters for the cache or to request certain kinds of documents from the cache. The caching directives are specified in a comma-separated list. For example: ``Cache-control: no-cache``. The following table lists the important cache request directives that can be used by the client in its HTTP request:

    | S.N.          | Cache Request Directive and Description    |
    | ------------- |-------------|
    | 1             | `no-cache`. A cache must not use the response to satisfy a subsequent request without successful revalidation with the origin server. |
    | 2             | `no-store`. The cache should not store anything about the client request or server response.      |
    | 3             | `max-age` = seconds`. Indicates that the client is willing to accept a response whose age is not greater than the specified time in seconds.     |
    | 4             | `max-stale` [ = seconds ] Indicates that the client is willing to accept a response that has exceeded its expiration time. If seconds are given, it must not be expired by more than that time.     |
    | 5             | `min-fresh` = seconds. Indicates that the client is willing to accept a response whose freshness lifetime is not less than its current age plus the specified time in seconds.     |
    | 6             | `no-transform`. Does not convert the entity-body.    |


----
### Opcache configuration

- [ ] **First find total amount of PHP files.** ``` $ find project/ -iname *.php|wc -l ```
- [ ] **Put all together**

    ```opcache.memory_consumption=128 [comment]: <> (# MB, adjust to your needs)
    opcache.max_accelerated_files=10000
    opcache.max_wasted_percentage=10
    opcache.validate_timestamps=0 ```


----

### Optimizing SSL on nginx
- [ ] Patch LOGJAM Vulnerability
- [ ] HTTP Strict Transport Security header.

    ```add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"```

- [ ] HTTP/2 - In your site’s nginx configuration file add ```listen 443 ssl http2;``` to the end of the listen line for the server block







----

### Server testing
- [ ] [SSL Labs] (https://www.ssllabs.com/ssltest/)
- [ ] [Mozilla Observatory] (https://observatory.mozilla.org/) :100:
- [ ] [Security Headers] (https://securityheaders.io/)





## Contributions wanted


## Feedback

All idea's, feature requests, pull requests, feedback, etc., are welcome. [Create an issue](https://github.com/johanneslamers/Deployment-guide-for-developers/issues).

## License

MIT License: see [the `LICENSE` file](https://github.com/johanneslamers/Deployment-guide-for-developers/blob/master/LICENSE).
