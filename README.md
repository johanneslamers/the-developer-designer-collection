#  :sparkles: Ultimate developer cheatsheet  :sparkles:

This is a collection of best practices, deployment tips, server management and security tips for developers & designers

## The Security Checklist

### Security
- [ ] **Install LetsEncrypt SSL**
- [ ] **Use HTTPS everywhere**
- [ ] **Patch LOGJAM Vulnerability**
- [ ] **Set HTTP Security Headers**

-------------

    #### HTTP Security Headers ###

    - [ ] **X-Frame-Options header.** This is an HTTP header that allows sites control over how your site may be framed within an iframe.
    - [ ] **Content Security Policy (CSP) header.** :100:

        ````
        Content-Security-Policy "default-src https: data: 'unsafe-inline' 'unsafe-eval'" always;

        ````

        **Examples**

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

        ````
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

        ````
    - [ ] **X-Content-Type-Options header.** This tells not to load scripts and stylesheets unless the server indicates the correct MIME type
    - [ ] **X-XSS-Protection header.** Stops pages from loading when they detect reflected cross-site scripting (XSS) attacks.

        ````
        X-XSS-Protection: 1; mode=block

        ````


----

### HTTP Cache Headers
- [ ] **Cache-Control**

    The Cache-Control general-header field is used to specify directives that MUST be obeyed by all the caching system. The syntax is as follows:
    An HTTP client or server can use the Cache-control general header to specify parameters for the cache or to request certain kinds of documents from the cache. The caching directives are specified in a comma-separated list. For example: ``Cache-control: no-cache``. The following table lists the important cache request directives that can be used by the client in its HTTP request:

    | #             | Cache Request Directive and Description    |
    | ------------- |-------------|
    | 1             | `no-cache`. A cache must not use the response to satisfy a subsequent request without successful revalidation with the origin server. |
    | 2             | `no-store`. The cache should not store anything about the client request or server response.      |
    | 3             | `max-age` = seconds`. Indicates that the client is willing to accept a response whose age is not greater than the specified time in seconds.     |
    | 4             | `max-stale` [ = seconds ] Indicates that the client is willing to accept a response that has exceeded its expiration time. If seconds are given, it must not be expired by more than that time.     |
    | 5             | `min-fresh` = seconds. Indicates that the client is willing to accept a response whose freshness lifetime is not less than its current age plus the specified time in seconds.     |
    | 6             | `no-transform`. Does not convert the entity-body.    |
- [ ] **Pragma**

    The old “pragma” header accomplishes many things most of them characterised by newer implementations. We are however most concerned with the `pragma: no-cache` directive which is interpreted by newer implementations as `cache-control: no-cache`. You don’t need to be concerned about this directive because it’s a request header which will be ignored by KeyCDN’s edge servers. It is however important to be aware of the directive for the overall understanding. Going forward, there won’t be new HTTP directives defined for pragma.
- [ ] **Expires**

    A couple of years back, this was the main way of specifying when assets expires. Expires is simply a basic date-time stamp. It’s fairly useful for old user agents which still roam unchartered territories. It is however important to note that cache-control headers, max-age and s-maxage still take precedence on most modern systems. It’s however good practice to set matching values here for the sake of compatibility. It’s also important to ensure you format the date properly or it might be considered as expired.

    ````
    Expires: Sun, 03 May 2015 23:02:37 GMT

    ````

    To avoid breaking the specification, avoid setting the date value to more than a year.

----

### PHP Opcache
- [ ] **First find total amount of PHP files for 'opcache.max_accelerated_files'**

    ````
    $ find project/ -iname *.php|wc -l

    ````
- [ ] **Change OpCache configuration**

    ````
    opcache.memory_consumption=128
    opcache.max_accelerated_files=10000
    opcache.max_wasted_percentage=10
    opcache.validate_timestamps=0

    ````



## Nginx

----

### Optimizing SSL on nginx
- [ ] **Patch LOGJAM Vulnerability**
- [ ] **HTTP/2** - In your site’s nginx configuration file add `listen 443 ssl http2;` to the end of the listen line for the server block
- [ ] **HTTP Strict Transport Security header** :100:

    ````
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"

    ````
- [ ] **Connection Credentials Caching**

    ````
    ssl_session_cache shared:SSL:20m;
    ssl_session_timeout 180m;

    ````
    > This will create a cache shared between all worker processes. The cache size is specified in bytes (in this example: 20 MB). According to the Nginx documentation can 1MB store about 4000 sessions, so for this example, we can store about 80000 sessions, and we will store them for 180 minutes. If you expect more traffic, increase the cache size accordingly.

- [ ] **Disable SSL** (What? Yes!)

    Techically SSL (Secure Sockets Layer) is actually superseded by TLS (Transport Layer Security). I guess it is just out of old habit and convention we still talk about SSL. SSL contains several weaknesses, there have been various attacks on implementations and it is vulnerable to certain protocol downgrade attacks.

    The only browser or library still known to mankind that doesn’t support TLS is of course IE 6. Since that browser is dead (should be, there is not one single excuse in the world), we can safely disable SSL.

    The most recent version of TLS is 1.2, but there are still modern browsers and libraries that use TLS 1.0.

    So, we’ll add this line to our config then:

    ````
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

    ````
- [ ] **Optimizing the cipher suites**

    The cipher suites are the hard core of SSL/TLS. This is where the encryption happens, and I will really not go into any of that here. All you need to know is that there are very secure suits, there are unsafe suites and if you thought browser compatibility issues were big on the front-end, this is a whole new ballgame. Researching what cipher suites to use, what not to use and in what order takes a huge amount of time to research. Luckily for you, I’ve done it.

    First you need to configure Nginx to tell the client that we have a preferred order of available cipher suites:

    ````
    ssl_prefer_server_ciphers on;

    ````

    Next we have to provide the actual list of ciphers:

    ````
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';

    ````

    All of these suites use forward secrecy, and the fast cipher AES is the preferred one. You’ll lose support for all versions of Internet Explorer on Windows XP. Who cares?



### Nginx config
- [ ] **Enable gzip** Add this to your nginx config above the server part:

    ````
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    ````
- [ ] **Static file caching**

    ````
    location ~* \.(?:rss|atom)$ {
        expires 1h;
        add_header Cache-Control "public";
    }

    location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|svgz|mp4|ogg|ogv|webm|htc)$ {
        try_files $uri /index.php?$query_string;
        expires 1M;
        add_header Cache-Control "public";
        etag off;
        access_log off;
        log_not_found off;
    }

    location ~* \.(?:css|js)$ {
        try_files $uri /index.php?$query_string;
        expires 1y;
        add_header Cache-Control "public";
        etag off;
        access_log off;
        log_not_found off;
    }

    location ~* \.(?:ttf|ttc|otf|eot|woff)$ {
        try_files $uri /index.php?$query_string;
        expires 1y;
        add_header Cache-Control "public";
        etag off;
        access_log off;
        log_not_found off;
    }

    location ~ /\.ht {
        deny all;
    }

    ````





----

### Server testing
- [ ] [SSL Labs] (https://www.ssllabs.com/ssltest/)
- [ ] [Mozilla Observatory] (https://observatory.mozilla.org/) :100:
- [ ] [Security Headers] (https://securityheaders.io/)

### Good reads
- [ ] [Servers for hackers](https://serversforhackers.com/)



## Contributions wanted


## Feedback

All idea's, feature requests, pull requests, feedback, etc., are welcome. [Create an issue](https://github.com/johanneslamers/Deployment-guide-for-developers/issues).

## License

MIT License: see [the `LICENSE` file](https://github.com/johanneslamers/Deployment-guide-for-developers/blob/master/LICENSE).
